# Evidence walkthrough

## 1. Baseline private profile

The initial `LISTDSD` output shows `IBMUSER.SECLAB.PRIVATE.*` with `UNIVERSAL ACCESS: NONE` and no `H7USER` in the access list.

Relevant screenshots:

- `01_private_profile_baseline_uacc_none.png`

## 2. Denied access before remediation

`H7USER` attempts to browse `IBMUSER.SECLAB.PRIVATE.DATA` and receives an authorization failure. The evidence includes an `ICH408I` message showing insufficient access authority.

Relevant screenshots:

- `02_private_access_denied_before_ich408i_and_dslist.png`

## 3. Temporary READ access

`IBMUSER` grants `H7USER` READ access using a specific `PERMIT` against the sandbox profile. `SETROPTS GENERIC(DATASET) REFRESH` is executed afterwards.

Relevant screenshots:

- `03_permit_private_h7user_read_and_refresh.png`
- `04_private_profile_h7user_read_access_list.png`

## 4. Successful browse

After the access-list entry is present, `H7USER` can browse the private dataset.

Relevant screenshots:

- `05_h7user_private_browse_success.png`

## 5. Access removal

The temporary access is removed with `PERMIT ... DELETE`, followed by another generic DATASET refresh.

Relevant screenshots:

- `06_remove_private_h7user_read_and_refresh.png`
- `07_private_profile_after_access_removed.png`

## 6. Final denied state

After removal, `H7USER` is denied again. This proves the exception was temporary and the original protection state was restored.

Relevant screenshots:

- `08_private_access_denied_after_removal_ich408i.png`
- `09_final_private_authorization_failed.png`
