# SN-CLIENT-004: getReference() used in a client script

**Reported to the reviewer; does not block** (priority 3).

## What it means

A client script calls `g_form.getReference()`. It does so even with a callback; without one, SN-CLIENT-001 also blocks it as synchronous.

## Why it matters

`getReference()` sends the **whole referenced record** to the browser, every field, to read what is usually one or two values.

## How to fix it

- For a reference's display value: `g_form.getDisplayValue('caller_id')`.
- For other fields: a GlideAjax call to a Script Include that returns just those fields.

```javascript
var ga = new GlideAjax('UserLookup');
ga.addParam('sysparm_name', 'getLocation');
ga.addParam('sysparm_user', g_form.getValue('caller_id'));
ga.getXMLAnswer(function(locationId) {
    g_form.setValue('location', locationId);
});
```
