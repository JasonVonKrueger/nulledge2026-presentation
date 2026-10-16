# SN-CFG-003: Hardcoded email address

**Reported to the reviewer; does not block** (priority 3).

## What it means

A string in a script contains an email address. Addresses at `example.com` are ignored.

## Why it matters

The address is the same in every environment, so test instances can email real people. It also changes on a different schedule from code: people leave and mailboxes are renamed.

## How to fix it

- Send to **users or groups**, not addresses.
- Or read the address from a **system property** or a notification's recipient list.
