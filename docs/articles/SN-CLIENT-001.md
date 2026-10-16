# SN-CLIENT-001: Synchronous server call in a client-side script

**Blocks completion** (priority 2).

## What it means

A client-side script waits for the server **synchronously**, which freezes the browser until the answer arrives:

- `ga.getXMLWait()` (GlideAjax);
- `g_form.getReference(field)` **without** a callback;
- `gr.query()` **without** a callback on a client-side GlideRecord.

## Why it matters

The form is frozen for the whole round trip: no typing, no scrolling. On slow networks or busy instances that's seconds, every time the script runs. `getXMLWait()` also does not work in Service Portal.

## How to fix it

Use the asynchronous form, and put the code that needs the answer in the callback.

Before:

```javascript
var ga = new GlideAjax('UserLookup');
ga.addParam('sysparm_name', 'getManager');
ga.addParam('sysparm_user', newValue);
ga.getXMLWait();
g_form.setValue('u_manager', ga.getAnswer());
```

After:

```javascript
var ga = new GlideAjax('UserLookup');
ga.addParam('sysparm_name', 'getManager');
ga.addParam('sysparm_user', newValue);
ga.getXMLAnswer(function(answer) {
    g_form.setValue('u_manager', answer);
});
```

Likewise: `g_form.getReference('caller_id', callback)` and `gr.query(callback)`.

## Where it applies

Client Scripts, Catalog Client Scripts, UI Policies, UI Scripts and client UI Actions. In UI Actions, only `getXMLWait()` and `getReference()` are checked, because their scripts can also contain server code.
