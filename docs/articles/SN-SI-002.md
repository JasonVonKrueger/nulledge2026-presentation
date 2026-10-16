# SN-SI-002: Duplicate Script Include API name

**Blocks completion** (priority 2).

## What it means

Another **active** Script Include has the same API name as this one.

## Why it matters

When two active Script Includes share an API name, which one a caller gets is undefined. Code can switch between them unpredictably.

## How to fix it

Give each Script Include a unique name, or deactivate the one that shouldn't be used.
