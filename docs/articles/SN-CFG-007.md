# SN-CFG-007: Update set changes an out-of-box record

**Blocks completion** (priority 1).

## What it means

Your update set changes a record **delivered by ServiceNow**, from a plugin or a ServiceNow store application. Changes to your own applications' records are not reported.

## Why it matters

Once you change an out-of-box record, upgrades **skip** it: it stops getting ServiceNow's fixes and improvements, and someone has to review it after every upgrade.

## How to fix it

- **Extend instead of changing:** add your own Business Rule, Script Include or UI Policy alongside the out-of-box one.
- If the change is unavoidable, document why and agree it with the platform owners. Then request an exception on the finding with that reason (see the overview article).
- If it was accidental, **revert** the record to the out-of-box version.
