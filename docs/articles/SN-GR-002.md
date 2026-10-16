# SN-GR-002: GlideRecord delete with no query conditions

**Blocks completion** (priority 1). The review variant **SN-GR-002-R** (priority 3) reports deletes whose conditions could not be verified.

## What it means

Your script deletes records with a GlideRecord that has **no query conditions**, so it deletes **every record in the table**. This covers:

- `deleteMultiple()` on a GlideRecord with no `addQuery` / `addEncodedQuery`;
- `deleteRecord()` inside a `while (gr.next())` loop over a query with no conditions.

`setLimit()` does not count as a condition.

## Why it matters

A delete without conditions removes every row in the table, usually as the system user, and cannot be undone short of a restore. A condition that silently drops out has the same effect, which is what **SN-GR-002-R** looks for: a misspelled field name, an empty value, or a condition added on only some code paths.

## How to fix it

Add conditions that limit the delete to the records you mean, and check the count first.

Before:

```javascript
var gr = new GlideRecord('u_import_staging');
gr.deleteMultiple();
```

After:

```javascript
var gr = new GlideRecord('u_import_staging');
gr.addQuery('u_batch', batchId);
gr.addQuery('u_processed', true);
gr.deleteMultiple();
```

## Not reported

- Deletes with at least one condition written in the code, such as `addQuery('parent', current.getUniqueValue())`.
- `deleteRecord()` after a single `if (gr.get(id))` or `if (gr.next())`.
- Comments and strings that mention `deleteMultiple()`.

## When it is intentional

If deleting every record really is intended, for example a staging table that is emptied on purpose, request an exception on the finding with that reason (see the overview article).
