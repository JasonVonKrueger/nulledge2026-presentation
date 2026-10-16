# SN-GR-003: GlideRecord update with no query conditions

**Blocks completion** (priority 2). The review variant **SN-GR-003-R** (priority 3) reports updates whose conditions could not be verified.

## What it means

Your script updates records with a GlideRecord that has **no query conditions**, so it changes **every record in the table**:

- `updateMultiple()` on a GlideRecord with no conditions;
- `update()` inside a `while (gr.next())` loop over a query with no conditions.

`setLimit()`, `setWorkflow(false)` and `autoSysFields(false)` do not count as conditions.

## Why it matters

An unrestricted update overwrites data across the whole table. Unless workflow is switched off, it also runs every update Business Rule, notification and flow for every record. **SN-GR-003-R** catches conditions that may silently drop out: run-time values, misspelled field names, or conditions added on only some code paths.

## How to fix it

Before:

```javascript
var gr = new GlideRecord('incident');
gr.setValue('state', 7);
gr.updateMultiple();
```

After:

```javascript
var gr = new GlideRecord('incident');
gr.addQuery('state', 6);
gr.addQuery('resolved_at', '<', gs.daysAgoStart(30));
gr.setValue('state', 7);
gr.updateMultiple();
```

Check the count with GlideAggregate before running a bulk update.

## Not reported

- Updates with at least one condition written in the code.
- `update()` after `get()` or a single `if (gr.next())`.
- `current.update()` (the Business Rule rules handle that).

## When it is intentional

For a deliberate one-off data fix, prefer a reviewed Fix Script, and request an exception on the finding with the reason (see the overview article).
