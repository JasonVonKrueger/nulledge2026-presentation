# SN-CLIENT-003: Direct DOM manipulation in a client script

**Reported to the reviewer; does not block** (priority 3).

## What it means

A client script reaches into the page directly:

- `document.getElementById()` / `querySelector()`;
- `gel()`, `$()`, `$j()` or `jQuery()`;
- setting `innerHTML` or `style`.

## Why it matters

The page's HTML is not an API. It changes between releases and UIs, so DOM code breaks on upgrade. It also doesn't work in Service Portal, Workspace or the mobile apps.

## How to fix it

Use the form APIs instead:

| Instead of | Use |
|---|---|
| hiding an element | `g_form.setDisplay(field, false)` / `setVisible` |
| colouring or marking a field | `g_form.addDecoration()`, `g_form.showFieldMsg()` |
| reading or writing values | `g_form.getValue()` / `setValue()` |
| making a field read-only | `g_form.setReadOnly()` |

## Not reported

`g_form.getControl()` is a supported API.
