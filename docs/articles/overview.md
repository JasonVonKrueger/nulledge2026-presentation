# Enterprise Coding Standard: how update sets are checked

When you complete an update set with **Scan & Complete**, ServiceNow scans everything in it against the Enterprise Coding Standard. This article explains what happens, how to read the results, and what to do about them.

## What happens when you click Scan & Complete

1. Instance Scan runs the standard's checks against every record in your update set: scripts, Business Rules, client scripts, Script Includes, REST resources and the update set itself.
2. **Priority 1 (Critical) or 2 (High) findings block completion.** The update set stays In Progress, and the message lists what was found.
3. **Priority 3 (Moderate) and 4 (Low) findings do not block.** They are attached to the peer review so the reviewer can look at them.
4. If nothing blocks, a **peer review** task is created for the Peer Reviewer group.

## Reading a finding

Each finding names a rule, such as `SN-GR-002`, and the record it was found in. For script rules it also gives the line numbers. Every rule has its own article in this knowledge base; the finding's documentation link opens it. The article explains what the rule looks for, why it matters, and how to fix it, with an example.

## When a finding is wrong, or the code is intentional

The rules are designed to avoid false alarms, but no automated check knows your intent.

- **Fix the code** if the finding is right. That's the normal case.
- **Request an exception** if the code is deliberate. See below.
- **Tell the owners of the standard** if a rule is wrong in general. Rules are improved when real false positives are reported.

## Requesting an exception

If a blocking finding is deliberate, ask the Peer Reviewer group to let it through:

1. On the update set, open the **Scan Findings** tab and open the finding.
2. Click **Request exception**, and explain why it is needed. Write for the reviewer: what the code does, why the rule does not apply, and what limits the risk ("Staging table, emptied nightly by design").
3. The Peer Reviewer group is notified. A member other than you approves or rejects it; a rejection comes with a comment. You are notified either way.
4. Once it is approved, run **Scan & Complete** again. The finding no longer blocks, and the peer review lists it under approved exceptions.

An exception covers that finding in that update set, exactly as it was when you asked. If you change the code and the finding changes with it (even just its line numbers), it blocks again and needs a new request. Your requests are under **Enterprise Coding Standard > Exceptions**.

## Protected components

Some components are designed a certain way and may only be changed by the group that owns them. If your update set changes one and you are not in its owner group, completion is blocked by `SN-CFG-008`. Ask the owner group to make the change, or request an exception if you have a legitimate reason to change it yourself. The list is maintained in **ECS Protected Component**; the SN-CFG-008 article explains how administrators secure it.

## The rules

{{RULE_INDEX}}

## Questions

Contact the owners of the Enterprise Coding Standard, or the Peer Reviewer group.
