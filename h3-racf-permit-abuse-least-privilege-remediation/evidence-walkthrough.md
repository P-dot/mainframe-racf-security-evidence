# Evidence Walkthrough

## Screenshot sequence

| # | Screenshot | What it shows |
|---|---|---|
| 01 | `01_listdsd_granted_profile_top_baseline.png` | `GRANTED` profile top section with `UACC(NONE)` |
| 02 | `02_listdsd_granted_access_list_user2_read_baseline.png` | Baseline access list showing `USER2 READ` |
| 03 | `03_permit_user2_alter_command_history_profile_top.png` | Command history showing the temporary `ACCESS(ALTER)` test command |
| 04 | `04_permit_alter_then_read_command_history_profile_top.png` | Command history showing both elevation and remediation commands |
| 05 | `05_listdsd_granted_access_list_user2_read_remediated.png` | Access list back to `USER2 READ` |
| 06 | `06_listdsd_public_uacc_read_final.png` | `PUBLIC` profile remains `UACC(READ)` for H2 exposure scenario |
| 07 | `07_listdsd_public_no_standard_access_list.png` | `PUBLIC` profile has no standard access list entries |
| 08 | `08_listdsd_private_uacc_none_final.png` | `PRIVATE` profile remains `UACC(NONE)` |
| 09 | `09_listdsd_private_no_standard_access_list.png` | `PRIVATE` profile has no standard access list entries |
| 10 | `10_listdsd_granted_uacc_none_final.png` | `GRANTED` profile final top section with `UACC(NONE)` |
| 11 | `11_listdsd_granted_access_list_user2_read_final.png` | Final access list shows `USER2 READ` |
| 12 | `12_search_dataset_ibmuser_seclab_no_entries.png` | Final SEARCH command result as captured |

## Evidence note

The final state is verified clearly. The transient `USER2 ALTER` state is captured in command history, but the access-list line showing `USER2 ALTER` was not captured before it was remediated. This is explicitly retained as a documentation lesson for future labs.
