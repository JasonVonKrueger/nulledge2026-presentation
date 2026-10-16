# SN-BR-002: Potential recursive Business Rule update

**Reported to the reviewer; does not block** (priority 3).

## What it means

A Business Rule writes records in a way that **can trigger the same rule again**:

- an **async** rule that runs on update calls `current.update()` without `current.setWorkflow(false)` first;
- the rule updates or inserts records **on its own table**, or a table that extends it, without `setWorkflow(false)` on that GlideRecord.

## Why it matters

A rule that triggers itself can loop, duplicate notifications and flows, and slow every save on the table.

## How to fix it

```javascript
(function executeRule(current, previous) {
    current.setValue('u_synced', true);
    current.setWorkflow(false); // don't run Business Rules again for this save
    current.update();
})(current, previous);
```

Or add a condition so the rule doesn't match its own write.

## Why only a warning

The check can't evaluate the rule's conditions. If your conditions already prevent recursion, explain that to your reviewer.
