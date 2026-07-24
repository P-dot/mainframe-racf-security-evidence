# Next Steps

## Complete zFS backing dataset protection review

For each exact aggregate shown by `/D OMVS,F`, run:

```text
LISTDSD DA('<zfs.aggregate.name>') ALL
```

Examples based on visible evidence:

```text
LISTDSD DA('DFH410.ZFS') ALL
LISTDSD DA('DSN910.SJVAZFS') ALL
LISTDSD DA('DSN910.SDSNGLS') ALL
```

Only use exact names confirmed from the display.

## Review broader generic DATASET coverage

Future checks:

```text
SEARCH CLASS(DATASET) MASK(**)
```

This can be very large. Use carefully.

## Connect with Health Checker

Compare these findings with the `RACF_SENSITIVE_RESOURCES` Health Checker output from the earlier lab.

## Do not harden blindly

Do not run commands such as `ADDSD`, `PERMIT`, `RALTER`, or `SETROPTS PROTECTALL` until there is a recovery plan, RACF database backup, and clear change scope.
