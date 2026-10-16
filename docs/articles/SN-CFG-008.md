# SN-CFG-008: Change to a protected component

**Blocks completion** (priority 1).

## What it means

Your update set changes (or deletes) a component that is on the **protected components** list, and you are not a member of its owner group. Protected components are scripts or configuration designed a certain way, whose changes must come from, or be approved by, the group that owns them. They are not necessarily out-of-box records.

The finding names the component, its owner group, the reason it is protected, and who made the change.

## Why it matters

Protected components are usually shared foundations, such as gate logic, integration utilities or security-sensitive rules. A well-meant change by someone who doesn't know the design can break everything that depends on it.

## How to fix it

- **Ask the owner group** to make the change.
- If you have a legitimate reason to change it yourself, **request an exception**: open the finding and click **Request exception** (see the overview article). The Peer Reviewer group decides.
- If the change was not intended, **back it out** of your update set.

A change made by a member of the owner group is not reported.

## For administrators

The list lives in **ECS Protected Component** (role `u_ecs_protected_component_user`). Each entry has:

| Field | Meaning |
|---|---|
| Table + Component | The protected record. Any type works, and matching is by sys_id, so renames don't matter. |
| Owner group | Its members may change the component. Leave it empty to report every change. |
| Reason | Shown in the finding. |
| Active | Untick to stop protecting it. |

The list is maintained on each instance; it does not travel in update sets.

## Securing the list (one-time setup)

Only the people who own the list should be able to change it. The standard creates the role `u_ecs_protected_component_user`, but the table's access controls (ACLs) must be created by hand. ServiceNow only allows ACLs to be created with the **security_admin** elevation, which automated installs cannot use. Until these four ACLs exist, the table relies on the instance's default access rules, and the menu's role setting only hides the module.

1. Make **ECS 1.0 - Enterprise Coding Standard** your current update set, so the ACLs travel with the standard.
2. Elevate to **security_admin** (user menu > Elevate role).
3. Go to **System Security > Access Control (ACL)** and create four ACLs, one for each operation: **read**, **create**, **write** and **delete**.

| Field | Value |
|---|---|
| Type | record |
| Operation | read / create / write / delete |
| Name | ECS Protected Component [u_ecs_protected_component], field --None-- |
| Decision type | Allow |
| Admin overrides | checked |
| Active | checked |
| Requires role | u_ecs_protected_component_user |

4. Give `u_ecs_protected_component_user` to the people who maintain the list.

Checks still see every entry: Instance Scan reads the table without applying ACLs.
