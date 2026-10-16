# SN-CFG-002: Hardcoded sys_id

**Reported to the reviewer; does not block** (priority 3).

## What it means

A string in a script is a **sys_id** (32 hexadecimal characters). The finding shows the value.

## Why it matters

A sys_id ties the script to one specific record. If that record doesn't exist on the target instance, or has a different sys_id there, the script silently does nothing or the wrong thing. It also hides which record is meant.

## How to fix it

```javascript
// Before
current.setValue('assignment_group', 'd625dccec0a8016700a222a0f7900d06');

// After: look it up, or keep it in a property set per environment
current.setValue('assignment_group', gs.getProperty('x.network.assignment_group'));
```

## When it is intentional

Records delivered in the same update set keep their sys_id on every instance. Tell your reviewer when that's the case.
