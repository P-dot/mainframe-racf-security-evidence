# Evidence Manifest

## Source document

```text
evidence/source-documents/LAB13.docx
```

## Screenshots

| File | Description |
|---|---|
| `01_search_class_program_profiles.png` | RACF `SEARCH CLASS(PROGRAM) MASK(*)` returned visible PROGRAM profiles. |
| `02_display_prog_apf_active_list.png` | SDSF console display `/D PROG,APF` showing APF-authorized libraries. |
| `03_display_prog_lnklst_active_list.png` | SDSF console display `/D PROG,LNKLST` showing active LINKLIST libraries. |
| `04_display_prog_lpa_syntax_error.png` | Attempted LPA display returned syntax error. |
| `05_listdsd_sys1_lnkllib_typo_no_profile.png` | `LISTDSD` for `SYS1.LNKLIB` typo returned no RACF description. |
| `06_listdsd_sys1_svclib_no_profile.png` | `LISTDSD` for `SYS1.SVCLIB` returned no RACF description. |
| `07_listdsd_sys1_lpalib_no_profile.png` | `LISTDSD` for `SYS1.LPALIB` returned no RACF description. |
| `08_listdsd_adcd_z111s_linklib_no_profile.png` | `LISTDSD` for `ADCD.Z111S.LINKLIB` returned no RACF description. |
| `09_listdsd_cee_sclbdll_no_profile.png` | `LISTDSD` for `CEE.SCLBDLL` returned no RACF description. |
| `10_listdsd_cee_sceerun_no_profile.png` | `LISTDSD` for `CEE.SCEERUN` returned no RACF description. |
| `11_search_dataset_sys1_generic_no_entries.png` | `SEARCH CLASS(DATASET) MASK(SYS1.*)` returned no matching entries. |
