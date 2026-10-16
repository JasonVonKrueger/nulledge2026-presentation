# SN-CFG-001: Hardcoded URL of this instance

**Blocks completion** (priority 2). The review variant **SN-CFG-001-R** (priority 3) reports other ServiceNow instance hosts.

## What it means

A string in a script contains **this instance's own address**, for example `https://acme.service-now.com/nav_to.do?...`, or a custom domain.

**SN-CFG-001-R** reports any **other** `*.service-now.com` or `*.servicenowservices.com` host in a string.

## Why it matters

Instance addresses differ between development, test and production, and clones copy scripts across. A hardcoded address keeps pointing at the original instance: links in test open production records, and integrations call the wrong environment.

## How to fix it

Build this instance's URL at run time:

```javascript
// Before
var link = 'https://acme.service-now.com/incident.do?sys_id=' + id;

// After
var link = gs.getProperty('glide.servlet.uri') + 'incident.do?sys_id=' + id;
```

For other instances or endpoints, use a Connection & Credential alias, a REST Message endpoint, or a system property set per environment.

## Not reported

- URLs in comments.
- `docs.servicenow.com`.
- Hosts an administrator lists in `ecs.cfg.allowed_hosts` (default `hi.service-now.com`).
