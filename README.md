# nullEDGE 2026: Stop Bad Code at the Door

Companion repository for my **nullEDGE 2026** presentation on using ServiceNow **Instance Scan** as a quality gate for development. It contains the update sets, checks and documentation shown in the talk, so you can install them, try them, or borrow ideas for your own instance.

**What it does:** when a developer finishes an update set and clicks **Scan & Complete**, the platform runs the Enterprise Coding Standard (ECS) checks against everything in that update set:

- Problems of priority 1–2 block completion. Each finding says what is wrong, on which line, and links to an article explaining the fix.
- Lower-priority findings go to a peer reviewer.
- A clean update set gets a peer review task. Once the review passes, the update set can be deployed to the test instance.

Built and validated on **Zurich**, in the **global** scope. Install it on a non-production instance first.

> Slides: [link to the deck]


# ServiceNow Enterprise Coding Standard and Gated Promotion

Update sets that add an automated quality gate to ServiceNow development. When a developer finishes an update set and clicks **Scan & Complete**, the platform runs the Enterprise Coding Standard (ECS) checks against everything in that update set:

- Problems of priority 1–2 block completion. Each finding says what is wrong, on which line, and links to an article explaining the fix.
- Lower-priority findings go to a peer reviewer.
- A clean update set gets a peer review task. Once the review passes, the update set can be deployed to the test instance.

Built and validated on **Zurich**, in the **global** scope.

## What's in this repository

| Path | Contents |
|---|---|
| `update-sets/[gate].xml` | **Gated promotion**: the Scan & Complete button, the Peer Review table and form, the Deploy button, and the logic that runs the scan and creates reviews |
| `update-sets/ECS 1.0 - Enterprise Coding Standard.xml` | **The coding standard**: 40 Instance Scan checks, their helper Script Includes, knowledge articles, exception requests, the dashboard and the application menu |
| `[other prerequisite update sets]` | [e.g. the GitSetGo exporter used for promotion] |
| `docs/` | Reference: the rules, and how the checks were validated |

## How it works

1. **The developer** clicks **Scan & Complete** on their update set. A progress dialog runs the checks in the "Update Set" scan suite, against only what that update set contains.
2. **If anything at priority 1–2 is found,** completion is blocked. The update set's **Scan Findings** tab lists every finding, with its priority, the record it's in and a link to the rule's article.
3. **If a finding is deliberate,** the developer opens it and clicks **Request exception**, with a justification. A member of the **Peer Reviewer** group (not the requester) approves or rejects it. An approved exception lets that exact finding through. It blocks again if the code changes.
4. **When nothing blocks,** the update set is completed and a **peer review** task is assigned to Peer Reviewer. It lists the remaining findings and any approved exceptions.
5. **The reviewer** passes or fails the review. **Deploy** sends the update set to the test instance for preview.

## The rules

40 checks across eight areas. Each rule has a knowledge article in the **Development Standards** knowledge base, linked from its findings.

| Area | Rules | Examples |
|---|---|---|
| GlideRecord | 9 | Unfiltered deletes and updates, nested queries, missing setLimit |
| Security | 5 | Hardcoded credentials, eval, unauthenticated REST resources |
| Business Rules | 4 | current.update() in before rules, recursion, Global table |
| Client scripts | 4 | Synchronous calls, DOM access, getReference() |
| Portability | 5 | Hardcoded instance URLs, sys_ids, emails, users and groups |
| Update sets | 4 | Collisions with other update sets, out-of-box changes, protected components |
| Script Includes | 3 | Naming, duplicate names, missing descriptions |
| Code quality | 6 | Debug output, empty catches, deprecated JSON, complexity |

Priority 1–2 block completion; 3–4 are reported to the reviewer. Some rules have an `-R` variant at priority 3 for cases the check can't confirm, so uncertain findings are reviewed rather than blocked.

## Installing

**Requirements**
- The Instance Scan plugin (`com.glide.instance_scan`) is active.
- Admin rights, to commit the update sets.
- security_admin, to create the access controls (step 4).

**Steps**
1. **Commit the prerequisites:** [other prerequisite update sets, in order].
2. **Commit the gate:** [gate update set]. This creates the "Update Set" scan suite, the Peer Reviewer group, the Scan & Complete and Deploy buttons, and the system property `instance_scan.peer_review.suite`.
3. **Commit the standard:** **ECS 1.0 - Enterprise Coding Standard**. Its checks join the "Update Set" suite, so the gate must be installed first.
4. **Create the access controls.** Elevate to **security_admin** first, because ACLs can't be shipped in a way that installs them automatically:
   - **ECS Protected Component** (`u_ecs_protected_component`): read, create, write and delete, each requiring the role `u_ecs_protected_component_user`.
   - **ECS Exception** (`u_ecs_exception`): read with no role; write requiring `u_ecs_exception_approver`. Add no create or delete ACLs.
5. **Grant roles:**
   - `u_ecs_exception_approver` to the **Peer Reviewer** group.
   - `u_ecs_dashboard_user` to lead developers, plus `scan_user` if they should open scan results.
   - `u_ecs_protected_component_user` to whoever maintains the protected list.
6. **Set up instance data:**
   - List your **protected components** (ECS menu → Protected Components): the scripts and configuration that only their owner group may change.
   - Optionally set `ecs.dashboard.excluded_update_sets` to hide test update sets from the dashboard.

## Using it

Everything is under **Enterprise Coding Standard** in the application navigator.

| Module | For |
|---|---|
| Dashboard | Lead developers: gate outcomes by week, first-scan pass rate, rules broken most, results by developer, waiting reviews and exception requests |
| Overview / Articles | Everyone: how the gate works and one article per rule |
| Findings, Scan Results | What the checks found |
| Exceptions | Requests to let a deliberate finding through, and their decisions |
| Checks, Gate Suite, Properties | Administrators: the standard's configuration |
| Protected Components | Owners of critical components |

**Developers:** you can check your work before you finish by running a point scan on a record or on your update set at any time. If a rule is wrong in general, rather than in your case, report it so the rule gets fixed.

## Configuration

| Property | Default | Purpose |
|---|---|---|
| `instance_scan.peer_review.suite` | — | The scan suite the gate runs |
| `instance_scan.peer_review.blocking_priorities` | `1,2` | Finding priorities that block completion |
| `instance_scan.peer_review.group` | `Peer Reviewer` | Group that reviews update sets and decides exceptions |
| `instance_scan.peer_review.names.script_include` | — | Naming pattern for Script Includes (SN-SI-001) |
| `ecs.complexity.max` | `10` | Complexity limit (SN-COMPLEXITY-001) |
| `ecs.gr.small_tables` | — | Tables allowed to be read without a filter (SN-GR-001) |
| `ecs.cfg.allowed_hosts` | `hi.service-now.com` | Host names allowed in scripts (SN-CFG-001) |
| `ecs.js.assignable_globals` | — | Extra global names scripts may assign (JS-002) |
| `ecs.dashboard.excluded_update_sets` | — | Name fragments of update sets the dashboard ignores |

## Known limitations

- **Protected components and roles are instance data.** The protected list and group role grants don't travel in update sets; set them up on each instance.
- **Some layout changes need a UI save to be captured.** If you change the Peer Review form or the Scan Findings list layout, save it once through **Configure → Form/List Layout** with the right update set current.
- **Exceptions are deliberately narrow.** An exception covers one finding in one update set. If the record changes and the finding changes with it (line numbers included), a new request is needed.
- **Deploy is a placeholder.** It asks for confirmation, but the deployment to the test instance is not implemented yet.
- **Only tested on one instance.** The update sets have been validated on one Zurich development instance; install on a non-production instance first.

