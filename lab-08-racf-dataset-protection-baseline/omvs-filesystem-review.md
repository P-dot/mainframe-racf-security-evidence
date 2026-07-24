# OMVS Filesystem Review

## Command

```text
/D OMVS,F
```

## Visible result

The display shows multiple active `ZFS` entries with `RDWR` mode and mount paths under `/1113/usr/lpp/...`.

Visible examples include product-related zFS aggregates for:

- CICS TS 4.1, for example `DFH410.ZFS`.
- DB2 9.1, for example `DSN910.SJVAZFS` and `DSN910.SDSNGLS`.

## Interpretation

This is important because z/OS UNIX files ultimately sit on MVS datasets. A RACF security review must therefore check both sides:

1. The OMVS user identity and UNIX privileges.
2. The RACF DATASET profiles protecting the backing zFS datasets.

Lab 07 showed broad use of UID(0). Lab 08 shows active zFS filesystems. The next step is to run `LISTDSD` for each exact aggregate dataset name shown by `/D OMVS,F`.

## Limitation

The screenshot does not show the full list cleanly enough for a final complete filesystem inventory. A future lab should capture the display with scrollable output or route the command output to a cleaner evidence source.
