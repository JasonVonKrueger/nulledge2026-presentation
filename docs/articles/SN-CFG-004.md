# SN-CFG-004: Hardcoded user or group name

**Reported to the reviewer; does not block** (priority 3).

## What it means

A string in a script is exactly the name of an existing **group**, or the user name of an existing **user**, for example `'Service Desk'` or `'abel.tuter'`. Role names are not reported.

## Why it matters

Users and groups are data. They are renamed, deactivated, or missing on other instances, and a script that names them breaks silently.

## How to fix it

```javascript
// Before
current.assignment_group.setDisplayValue('Service Desk');

// After
current.setValue('assignment_group', gs.getProperty('x.default_assignment_group'));
```

Assignment rules, and configuration your team can change without a code release, are often better still.
