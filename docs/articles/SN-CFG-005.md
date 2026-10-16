# SN-CFG-005: Artifact also changed in another in-progress update set

**Blocks completion** (priority 2).

## What it means

A record changed in your update set is **also captured in another update set that is still in progress**. The finding lists each shared record and the other update set.

## Why it matters

Two people (or two pieces of work) are changing the same record. Whichever update set is committed last silently overwrites the other's change.

## How to fix it

- Talk to the owner of the other update set.
- Merge the work into one update set, or agree an order and complete them one after the other.
- If the other update set is stale, close or clean it up.

A common cause is work done while **Default** was the current update set. Check that you are in the right update set before you start.
