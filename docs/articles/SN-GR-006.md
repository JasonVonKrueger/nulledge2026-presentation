# SN-GR-006: getRowCount() used to count records

**Reported to the reviewer; does not block** (priority 3).

## What it means

`getRowCount()` is called on a GlideRecord whose rows are never read. The query loads every matching row into memory just to count them.

## How to fix it

Count in the database with GlideAggregate.

Before:

```javascript
var gr = new GlideRecord('incident');
gr.addActiveQuery();
gr.query();
var open = gr.getRowCount();
```

After:

```javascript
var ga = new GlideAggregate('incident');
ga.addActiveQuery();
ga.addAggregate('COUNT');
ga.query();
var open = ga.next() ? parseInt(ga.getAggregate('COUNT'), 10) : 0;
```

## Not reported

`getRowCount()` on a query whose rows you also read, for example for a progress message.
