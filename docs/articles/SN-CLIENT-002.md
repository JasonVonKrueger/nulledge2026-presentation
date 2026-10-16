# SN-CLIENT-002: onChange client script without a load/empty guard

**Reported to the reviewer; does not block** (priority 3).

## What it means

An **onChange** client script never checks `isLoading`, or never checks for an empty `newValue`.

## Why it matters

The platform calls `onChange` when the form **loads** and when the field is **cleared**, not only when a user changes the value. Without the guard, the script runs server calls on every form load and can overwrite values nobody touched.

## How to fix it

Start the function with the standard guard:

```javascript
function onChange(control, oldValue, newValue, isLoading, isTemplate) {
    if (isLoading || newValue === '') {
        return;
    }
    // your logic
}
```

If you really need to react to the field being cleared, check `newValue === ''` explicitly and handle it.

## Not reported

- Guards written differently: `!newValue`, `'' == newValue`, or renamed parameters.
- A guard in a helper function inside `onChange`.
