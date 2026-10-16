# SN-BR-001: Unnecessary current.update() in a Business Rule

**Blocks completion** (priority 2).

## What it means

A **before**, **after** or **display** Business Rule calls `current.update()`.

- **before:** changes to `current` are saved automatically when the rule finishes. `update()` saves the record twice and runs this table's Business Rules again.
- **after:** the record has already been saved, so `update()` saves it a second time and runs the rules again.
- **display:** `update()` saves the record every time someone opens the form.

## Why it matters

Double saves cost performance, duplicate notifications and flows, clutter the audit history, and can loop until the platform's recursion guard stops them.

## How to fix it

In a **before** rule, just set the fields:

```javascript
(function executeRule(current, previous) {
    current.setValue('short_description', 'Updated');
    // no current.update() here; the save persists it
})(current, previous);
```

If you need to change fields **after** the save, move the change to a before rule. In a **display** rule, never save the record.

## Not reported

- Async Business Rules: saving `current` can be legitimate there (SN-BR-002 checks them for recursion).
- `current.update()` outside Business Rules, such as in a UI Action.
- Comments and strings.
