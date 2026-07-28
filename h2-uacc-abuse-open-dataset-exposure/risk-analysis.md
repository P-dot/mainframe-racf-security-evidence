# Risk Analysis

## Risk: Information disclosure through UACC(READ)

A dataset profile with `UACC(READ)` can allow broad read access. In production, this can expose system or application information even when no explicit access-list entry exists.

## Impact

Possible impact includes:

- leakage of JCL or operational design
- exposure of dataset naming standards
- disclosure of subsystem/application structure
- reconnaissance value for later misuse
- compliance and data-governance failure if sensitive data is exposed

## Severity in this lab

Low, because only benign sandbox datasets are involved.

## Severity in production

Potentially medium to high, depending on the data and operational value of the dataset.

## Root cause pattern

The root cause is not a command failure. The root cause is policy design:

```text
open universal access instead of closed-by-default access
```
