# Lab 08 — RACF Dataset Protection Baseline

## Purpose

This lab documents a read-only RACF review of sensitive z/OS system datasets and z/OS UNIX mounted file systems in an ADCD z/OS 1.11 lab environment.

The lab continues the previous sequence:

- Lab 06: technical RACF users and privilege review.
- Lab 07: z/OS UNIX / OMVS UID(0) baseline.
- Lab 08: dataset protection baseline for system libraries and mounted zFS datasets.

## Scope

This lab reviewed RACF DATASET profiles for:

- `ADCD.Z111S.PARMLIB`
- `ADCD.Z111S.PROCLIB`
- `ADCD.Z111S.LINKLIB`
- `ADCD.Z111S.VTAMLIB`
- `SYS1.PARMLIB`
- `SYS1.PROCLIB`
- `SYS1.LINKLIB`
- `SYS1.LPALIB`
- `SYS1.NUCLEUS`

It also captured the output of `/D OMVS,F` to identify active zFS file systems.

## Key conclusion

The evidence shows no RACF DATASET profiles found for the reviewed system libraries and no matching generic DATASET profiles for `ADCD.Z111S.*` or `SYS1.*` from the executed searches.

This is consistent with a lab ADCD system, but in a production hardening context it would be a high-priority review item.

## Safety

No RACF changes were performed. This was an evidence-only review.
