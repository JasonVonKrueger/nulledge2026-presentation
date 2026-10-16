# SN-COMPLEXITY-001: Excessive function complexity

**Informational; does not block** (priority 4).

## What it means

A function has too many decision points. Each `if`, `else if`, `case`, loop, `catch`, `?:`, `&&` and `||` counts. The limit is 10, and administrators can change it with `ecs.complexity.max`.

## Why it matters

Every decision doubles the paths someone has to understand and test. Complex functions are where bugs hide and where changes break things.

## How to fix it

- **Split** the function into smaller ones, each with one job.
- Replace long `if`/`else` or `switch` chains with a **lookup object**.
- **Return early** instead of nesting.

```javascript
// Before: a switch with many cases
// After:
var PRIORITY_BY_IMPACT = { '1': 1, '2': 2, '3': 3 };
return PRIORITY_BY_IMPACT[impact] || 4;
```
