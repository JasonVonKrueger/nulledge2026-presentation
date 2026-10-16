# SN-GR-001: GlideRecord query with no conditions

**Reported to the reviewer; does not block** (priority 3).

## What it means

A `query()` runs on a GlideRecord (or GlideRecordSecure) **with no conditions and no limit**, so it reads every record in the table.

## Why it matters

Reading a whole table is slow and memory-hungry on large tables, and gets worse as the table grows. Sometimes it is fine: a small configuration table, or a job that really must touch every record. That is why this is a warning for the reviewer, not a block.

## How to fix it

Add conditions for the records you need, or a limit.

Before:

```javascript
var gr = new GlideRecord('incident');
gr.query();
while (gr.next()) {
    // ...
}
```

After:

```javascript
var gr = new GlideRecord('incident');
gr.addQuery('active', false);
gr.query();
while (gr.next()) {
    // ...
}
```

- To read **one** record, use `get()` or `setLimit(1)`.
- To **count**, use GlideAggregate.

## Not reported

- GlideAggregate, `get()`, and tables whose name is computed at run time.
- Queries with a condition, `setLimit()` or `chooseWindow()` before them.
- Queries whose conditions exist but could not be verified (the delete and update rules handle those).
- Tables an administrator has listed in the `ecs.gr.small_tables` system property.

## When it is intentional

If the table is small by design, ask an administrator to add it to `ecs.gr.small_tables`. Otherwise explain the reason to your reviewer.
