# SN-CFG-006: Update set reopened after completion

**Blocks completion** (priority 2).

## What it means

This update set was **Complete** at some point and was set back to **In Progress**.

## Why it matters

If it was already retrieved into another instance, the changes you made after reopening won't reach that instance unless the update set is completed and retrieved again. That is easy to miss.

## How to fix it

- Put new changes in a **new update set** instead of reopening a completed one.
- If reopening was necessary, make sure whoever moves update sets knows it must be retrieved and committed again everywhere.
