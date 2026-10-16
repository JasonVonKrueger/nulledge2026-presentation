# SN-GR-004: Query inside a query loop

**Reported to the reviewer; does not block** (priority 3).

## What it means

A GlideRecord `query()` or `get()` runs **inside the `while (x.next())` loop of another GlideRecord**. The inner query runs once for every record of the outer query, so 1,000 outer records mean 1,000 extra queries.

## Why it matters

This "N+1" pattern is one of the most common causes of slow scripts and timeouts, and it gets worse as data grows.

## How to fix it

Fetch what you need in one query before the loop, then look it up.

Before:

```javascript
var inc = new GlideRecord('incident');
inc.addActiveQuery();
inc.query();
while (inc.next()) {
    var user = new GlideRecord('sys_user');
    user.get(inc.getValue('caller_id'));
    // use user...
}
```

After:

```javascript
var callerIds = [];
var inc = new GlideRecord('incident');
inc.addActiveQuery();
inc.query();
while (inc.next()) {
    callerIds.push(inc.getValue('caller_id'));
}
var user = new GlideRecord('sys_user');
user.addQuery('sys_id', 'IN', callerIds.join(','));
user.query();
while (user.next()) {
    // use user...
}
```

Dot-walking (`inc.caller_id.email`) or GlideAggregate can also replace many inner queries.

## Not reported

- Queries on the loop's own GlideRecord.
- Queries before the loop.
- Loops that are not GlideRecord query loops, such as a `for` loop over a list of IDs.

## When it is intentional

Small, bounded outer queries can be fine. Explain the bound to your reviewer.
