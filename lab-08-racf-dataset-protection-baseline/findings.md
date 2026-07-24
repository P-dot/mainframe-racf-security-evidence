# Findings

## Finding 1 — No RACF profile found for ADCD system libraries

The following datasets returned `ICH35003I NO RACF DESCRIPTION FOUND`:

```text
ADCD.Z111S.PARMLIB
ADCD.Z111S.PROCLIB
ADCD.Z111S.LINKLIB
ADCD.Z111S.VTAMLIB
```

### Interpretation

These datasets contain sensitive system configuration, procedures, executable libraries, and VTAM-related libraries. In a hardened production system, they should normally be protected by explicit or generic RACF DATASET profiles with carefully controlled access.

### Evidence

See screenshots:

- `01_LISTDSD_ADCD_Z111S_PARMLIB_no_profile.png`
- `02_LISTDSD_ADCD_Z111S_PROCLIB_no_profile.png`
- `03_LISTDSD_ADCD_Z111S_LINKLIB_no_profile.png`
- `04_LISTDSD_ADCD_Z111S_VTAMLIB_no_profile.png`

## Finding 2 — No RACF profile found for SYS1 core libraries

The following datasets returned `ICH35003I NO RACF DESCRIPTION FOUND`:

```text
SYS1.PARMLIB
SYS1.PROCLIB
SYS1.LINKLIB
SYS1.LPALIB
SYS1.NUCLEUS
```

### Interpretation

These are classic system datasets. Their lack of visible RACF profiles in this lab evidence is an important baseline finding.

### Evidence

See screenshots:

- `05_LISTDSD_SYS1_PARMLIB_no_profile.png`
- `06_LISTDSD_SYS1_PROCLIB_no_profile.png`
- `07_LISTDSD_SYS1_LINKLIB_no_profile.png`
- `08_LISTDSD_SYS1_LPALIB_no_profile.png`
- `09_LISTDSD_SYS1_NUCLEUS_no_profile.png`

## Finding 3 — No generic DATASET profiles found for the searched masks

The following searches returned no matching entries:

```text
SEARCH CLASS(DATASET) MASK(ADCD.Z111S.*)
SEARCH CLASS(DATASET) MASK(SYS1.*)
```

The visible message was:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

### Interpretation

This supports the earlier LISTDSD results: no visible generic DATASET profiles were found for these high-level qualifiers using the executed masks.

### Evidence

See screenshots:

- `10_SEARCH_DATASET_ADCD_Z111S_no_entries.png`
- `11_SEARCH_DATASET_SYS1_no_entries.png`

## Finding 4 — Active zFS file systems are mounted

The `/D OMVS,F` output shows active zFS file systems mounted in the system. Visible examples include CICS and DB2-related zFS aggregates, including names such as:

```text
DFH410.ZFS
DSN910.SJVAZFS
DSN910.SDSNGLS
```

Some names are partially visible in the screenshot and should be rechecked with a wider/scrollable display before producing a complete filesystem inventory.

### Interpretation

This connects to Lab 07. Many technical IDs had UID(0), and this lab shows active z/OS UNIX file systems. The next hardening question is whether the backing zFS datasets are protected by RACF DATASET profiles.

### Evidence

See screenshot:

- `12_DISPLAY_OMVS_FILESYSTEMS.png`
