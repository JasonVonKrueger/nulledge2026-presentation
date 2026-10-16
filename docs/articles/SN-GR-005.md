# SN-GR-005: Single-record read without setLimit(1)

**Reported to the reviewer; does not block** (priority 3).

## What it means

A filtered `query()` reads **only one record** (`if (gr.next())`) but has no `setLimit(1)`. The database still prepares every matching row.

## How to fix it

Before:

```javascript
var user = new GlideRecord('sys_user');
user.addQuery('email', email);
user.query();
if (user.next()) {
    return user.getUniqueValue();
}
```

After:

```javascript
var user = new GlideRecord('sys_user');
user.addQuery('email', email);
user.setLimit(1);
user.query();
if (user.next()) {
    return user.getUniqueValue();
}
```

For a lookup by a unique field, `user.get('email', email)` is simpler.

## Not reported

- Queries that already have `setLimit()` or `chooseWindow()`.
- Rows read in a loop.
- `get()`.
- Unfiltered queries (SN-GR-001 reports those).
