# SN-BR-003: Display Business Rule runs queries

**Reported to the reviewer; does not block** (priority 3).

## What it means

A **display** Business Rule creates a GlideRecord, GlideAggregate or GlideQuery. Display rules run every time a form is opened, so every query in them slows down every form load.

## How to fix it

- Keep display rules to filling `g_scratchpad` from data already on `current`.
- Fetch other data only when it's needed, with a GlideAjax call from a client script.

Before:

```javascript
(function executeRule(current, previous) {
    var ga = new GlideAggregate('task_sla');
    ga.addQuery('task', current.getUniqueValue());
    ga.addAggregate('COUNT');
    ga.query();
    g_scratchpad.slaCount = ga.next() ? ga.getAggregate('COUNT') : 0;
})(current, previous);
```

After: move the count to a Script Include called through GlideAjax when the user needs it, or show it in a related list.
