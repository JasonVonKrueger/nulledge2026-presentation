# SN-SI-001: Script Include name does not follow the naming convention

**Reported to the reviewer; does not block** (priority 3).

## What it means

The Script Include's name doesn't match the naming convention set in the system property `instance_scan.peer_review.names.script_include`. On this instance that is `^[A-Z][a-zA-Z0-9]*$`: a capital letter followed by letters and digits, such as `IncidentUtils`.

## How to fix it

Rename the Script Include, and update every place that calls it. Renaming changes the class name, so search for the old name in scripts first.
