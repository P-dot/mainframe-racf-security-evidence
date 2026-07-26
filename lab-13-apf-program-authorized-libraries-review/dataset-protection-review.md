# Dataset Protection Review for Authorized Libraries

This section records `LISTDSD` and `SEARCH` checks captured during the lab.

## Captured results

| Evidence | Command | Captured result |
|---|---|---|
| `05_listdsd_sys1_lnkllib_typo_no_profile.png` | `LISTDSD DA('SYS1.LNKLIB') ALL` | `ICH35003I NO RACF DESCRIPTION FOUND FOR SYS1.LNKLIB` |
| `06_listdsd_sys1_svclib_no_profile.png` | `LISTDSD DA('SYS1.SVCLIB') ALL` | `ICH35003I NO RACF DESCRIPTION FOUND FOR SYS1.SVCLIB` |
| `07_listdsd_sys1_lpalib_no_profile.png` | `LISTDSD DA('SYS1.LPALIB') ALL` | `ICH35003I NO RACF DESCRIPTION FOUND FOR SYS1.LPALIB` |
| `08_listdsd_adcd_z111s_linklib_no_profile.png` | `LISTDSD DA('ADCD.Z111S.LINKLIB') ALL` | `ICH35003I NO RACF DESCRIPTION FOUND FOR ADCD.Z111S.LINKLIB` |
| `09_listdsd_cee_sclbdll_no_profile.png` | `LISTDSD DA('CEE.SCLBDLL') ALL` | `ICH35003I NO RACF DESCRIPTION FOUND FOR CEE.SCLBDLL` |
| `10_listdsd_cee_sceerun_no_profile.png` | `LISTDSD DA('CEE.SCEERUN') ALL` | `ICH35003I NO RACF DESCRIPTION FOUND FOR CEE.SCEERUN` |
| `11_search_dataset_sys1_generic_no_entries.png` | `SEARCH CLASS(DATASET) MASK(SYS1.*)` | `ICH31005I NO ENTRIES MEET SEARCH CRITERIA` |

## Evidence caveat

`SYS1.LNKLIB` appears to be a typo for `SYS1.LINKLIB`; therefore this specific screenshot is not used to make a new assertion about `SYS1.LINKLIB`. Earlier dataset-protection labs already reviewed `SYS1.LINKLIB`. In this lab, the typo is preserved transparently.

## Finding

```text
Several selected sensitive/APF/LINKLIST-related libraries reviewed in the captured evidence do not show visible RACF DATASET profiles.
```

## Why this matters

If a library is APF-authorized or participates in LINKLIST processing, protection of the backing dataset is critical. Unauthorized update access to such a library could allow modification or introduction of sensitive executable code.
