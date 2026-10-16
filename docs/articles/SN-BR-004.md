# SN-BR-004: Global Business Rule

**Blocks completion** (priority 2).

## What it means

A Business Rule's **Table** is **Global**. The platform loads global Business Rules for every server-side transaction, whether they're needed or not. They are an old way to share functions.

## How to fix it

- Move shared functions to a **Script Include**, which loads only when it's called.
- If the logic reacts to records, put the Business Rule on the specific table.

```javascript
// Script Include "IncidentHelpers"
var IncidentHelpers = Class.create();
IncidentHelpers.prototype = {
    initialize: function() {},
    isVip: function(userId) {
        var user = new GlideRecord('sys_user');
        return user.get(userId) && user.getValue('vip') === '1';
    },
    type: 'IncidentHelpers'
};
```
