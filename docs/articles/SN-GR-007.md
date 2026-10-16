# SN-GR-007: Field set by direct assignment

**Informational; does not block** (priority 4).

## What it means

A field is set by assigning to it, for example `current.state = 3;`, instead of with `setValue()`.

## Why it matters

`setValue()` is the documented API. It converts the value for the field's type, and it reads clearly as "set a field", not "set a JavaScript property".

## How to fix it

```javascript
// Before
current.state = 3;

// After
current.setValue('state', 3);
```

Use `setDisplayValue()` when you have a display value rather than the stored value.

## Not reported

Reading fields, including dot-walking (`current.caller_id.name`), is fine.
