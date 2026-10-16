# SN-INT-001: Outbound call without error handling

**Reported to the reviewer; does not block** (priority 3).

## What it means

A RESTMessageV2 or SOAPMessageV2 call (`execute()` / `executeAsync()`) is **not inside try/catch** and its **response status is never checked**.

## Why it matters

RESTMessageV2 doesn't throw on HTTP errors: a 401, 404 or 500 comes back as a normal response. Without a status check, the script treats an error page as data and the failure surfaces much later, if at all.

## How to fix it

```javascript
try {
    var request = new sn_ws.RESTMessageV2('Vendor API', 'get');
    var response = request.execute();
    if (response.haveError() || response.getStatusCode() != 200) {
        gs.error('Vendor API failed: ' + response.getStatusCode() + ' ' + response.getErrorMessage());
        return null;
    }
    return JSON.parse(response.getBody());
} catch (e) {
    gs.error('Vendor API call threw: ' + e);
    return null;
}
```

## Not reported

A response passed to another function, returned, or stored. The handling may happen there.
