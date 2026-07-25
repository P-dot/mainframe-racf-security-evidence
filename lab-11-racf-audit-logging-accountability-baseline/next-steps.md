# Next Steps

## Recommended next lab

Lab 12 - JES2, SDSF, OPERCMDS and Console Authority Review

## Why this comes next

After auditing RACF users, OMVS, zFS, SAF controls, and SMF baseline, the next sensitive area is operational control:

- who can view jobs and SYSOUT
- who can issue operator commands
- how SDSF access is controlled
- whether OPERCMDS profiles exist
- how JES2/SPOOL access is exposed

## Read-only evidence to collect next

```text
SEARCH CLASS(OPERCMDS) MASK(*)
SEARCH CLASS(SDSF) MASK(*)
RLIST STARTED SDSF ALL
/D OPDATA
/D JES2
/D A,L
```

Additional commands will be adjusted to the exact ADCD behavior.
