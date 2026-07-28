# Evidence Walkthrough

Source evidence: `H2.docx`, 9 pages.

## Page-level walkthrough

| Evidence | Screenshot | What it shows |
|---|---|---|
| 01 | `01_search_mask_no_entries_initial.png` | Initial `SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)`; search did not return entries, so `LISTDSD` is used to validate exact profiles. |
| 02-03 | `02_public_profile_uacc_read_top.png`, `03_public_profile_continuation_no_access_list.png` | `IBMUSER.SECLAB.PUBLIC.*` has `UACC(READ)` and no standard access-list entries. |
| 04-05 | `04_private_profile_uacc_none_top.png`, `05_private_profile_continuation_no_access_list.png` | `IBMUSER.SECLAB.PRIVATE.*` has `UACC(NONE)`. |
| 06-07 | `06_granted_profile_uacc_none_top.png`, `07_granted_access_list_user2_read.png` | `IBMUSER.SECLAB.GRANTED.*` has `UACC(NONE)` and `USER2 READ`. |
| 08 | `08_granted_dataset_content.png` | Benign granted dataset content explaining least privilege. |
| 09 | `09_public_dataset_content.png` | Benign public dataset content explaining open exposure. |
| 10 | `10_private_dataset_content.png` | Benign private dataset content explaining restriction. |
| 11-12 | `11_public_profile_final_uacc_read.png`, `12_public_profile_final_continuation.png` | Final review of the public profile. |
| 13 | `13_private_profile_final_uacc_none.png` | Final review of the private profile. |
| 14-16 | `14_granted_profile_final_uacc_none.png`, `15_granted_profile_final_dates_continuation.png`, `16_granted_access_list_user2_read_final.png` | Final review of the granted profile and `USER2 READ` access-list entry. |
| 17 | `17_listuser_user2_exists.png` | `USER2` exists in RACF. |
| 18 | `18_user2_not_authorized_to_use_tso.png` | `USER2` cannot log on to TSO in this environment, so live cross-user browse testing is not available yet. |

## Interpretation

The lab proves the policy difference by profile evidence, even though live testing as `USER2` could not be completed due to TSO authorization.
