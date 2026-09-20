# Part 2 Evidence Manifest

The screenshots in this directory are extracted from the user's captured z/OS lab session and are retained as direct evidence for the Part 2 conclusions.

| File | Evidence |
|---|---|
| `01-ftpjes2-jcl.png` | Minimal valid `FTPJES2` / `IEFBR14` member |
| `02-tso-submit-denied.png` | `H7USER` denied TSO/E `SUBMIT` |
| `03-ftp-jes-job07465-submitted.png` | FTP/JES internal-reader handoff and `JOB07465` assignment |
| `04-tsoauth-jcl-profile.png` | `TSOAUTH JCL` profile baseline |
| `05-h7user-no-privileged-attributes.png` | `LISTUSER H7USER`, `ATTRIBUTES=NONE` |
| `06-jesjobs-no-profiles.png` | `SEARCH CLASS(JESJOBS)` returns no entries |
| `07-job07465-identity-and-completion.png` | JES log assigns H7USER and shows job start/end |
| `08-job07465-jesjcl.png` | JESJCL confirms submitted two-line job |
| `09-job07465-cc0000.png` | JESYSMSG confirms `COND CODE 0000` |
| `10-jesinput-no-profiles.png` | `SEARCH CLASS(JESINPUT)` returns no entries |
| `11-surrogat-profile-search.png` | SURROGAT inventory includes `IBMUSER.SUBMIT` |
| `12-surrogat-ibmuser-submit-profile.png` | `IBMUSER.SUBMIT` profile baseline |
| `13-surrogat-no-h7user-access.png` | Access list shows no H7USER grant |
| `14-rdi-no-selectable-entries.png` | Point-in-time RDI display after job completion |
| `15-intrdr-batch-enabled.png` | `$D INTRDR` shows `BATCH=YES`, class A and current characteristics |

## Publication review

No password value is shown in the selected Part 2 evidence. The retained network reference is loopback (`127.0.0.1`) in the test flow. Before publication, continue to review screenshots for any unrelated terminal/session identifiers or environmental detail that is not necessary to the technical conclusion.
