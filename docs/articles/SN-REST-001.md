# SN-REST-001: Scripted REST resource without authentication

**Blocks completion** (priority 1).

## What it means

A Scripted REST resource has **Requires authentication** turned off, so anyone who can reach the instance can call it without logging in.

## How to fix it

On the resource:

1. Tick **Requires authentication**.
2. Control who may call it with ACLs, and tick **Requires ACL authorization**.

## When it is intentional

Some endpoints are deliberately public, such as a health check. Make sure it exposes nothing sensitive and accepts no input that changes data, then request an exception on the finding with the reason (see the overview article).
