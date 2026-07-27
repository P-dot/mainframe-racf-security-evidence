# Commands — H1 RACF Sandbox Dataset Profiles

All commands were executed from **ISPF option 6**.

## 1. Initial baseline

```text
SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)

LISTCAT ENT('IBMUSER.SECLAB.PUBLIC.DATA') ALL
LISTCAT ENT('IBMUSER.SECLAB.PRIVATE.DATA') ALL
LISTCAT ENT('IBMUSER.SECLAB.GRANTED.DATA') ALL
```

Purpose: prove that the sandbox did not already exist.

## 2. Dataset creation

```text
ALLOC DATASET('IBMUSER.SECLAB.PUBLIC.DATA') NEW CATALOG UNIT(SYSDA) SPACE(1,1) TRACKS DSORG(PS) RECFM(F B) LRECL(80) BLKSIZE(800)

ALLOC DATASET('IBMUSER.SECLAB.PRIVATE.DATA') NEW CATALOG UNIT(SYSDA) SPACE(1,1) TRACKS DSORG(PS) RECFM(F B) LRECL(80) BLKSIZE(800)

ALLOC DATASET('IBMUSER.SECLAB.GRANTED.DATA') NEW CATALOG UNIT(SYSDA) SPACE(1,1) TRACKS DSORG(PS) RECFM(F B) LRECL(80) BLKSIZE(800)
```

Purpose: create safe non-VSAM lab datasets.

## 3. Dataset catalog verification

```text
LISTCAT ENT('IBMUSER.SECLAB.PUBLIC.DATA') ALL
LISTCAT ENT('IBMUSER.SECLAB.PRIVATE.DATA') ALL
LISTCAT ENT('IBMUSER.SECLAB.GRANTED.DATA') ALL
```

Purpose: confirm that the datasets exist and are cataloged.

## 4. Syntax correction captured during the lab

Initial attempt:

```text
ADDSD 'IBMUSER.SECLAB.PUBLIC.**' GENERIC UACC(READ)
```

Result:

```text
IKJ56702I INVALID DATASET NAME
```

The command was corrected to use a non-enhanced generic pattern and no `GENERIC` keyword in `ADDSD`:

```text
ADDSD 'IBMUSER.SECLAB.PUBLIC.*' UACC(READ)
```

## 5. Public profile — intentionally open

```text
ADDSD 'IBMUSER.SECLAB.PUBLIC.*' UACC(READ)
LISTDSD DA('IBMUSER.SECLAB.PUBLIC.*') ALL
```

Expected result:

```text
UNIVERSAL ACCESS: READ
```

## 6. Private profile — closed by default

```text
ADDSD 'IBMUSER.SECLAB.PRIVATE.*' UACC(NONE)
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

Expected result:

```text
UNIVERSAL ACCESS: NONE
```

## 7. Granted profile — closed by default with explicit permit

```text
ADDSD 'IBMUSER.SECLAB.GRANTED.*' UACC(NONE)
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(READ)
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```

Expected result:

```text
UNIVERSAL ACCESS: NONE
USER2 READ
```

## 8. Audit on private profile

```text
ALTDSD 'IBMUSER.SECLAB.PRIVATE.*' GENERIC AUDIT(FAILURES(READ))
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
```

Expected result:

```text
AUDITING
FAILURES(READ)
```

## 9. Final comparison

```text
LISTDSD DA('IBMUSER.SECLAB.PUBLIC.*') ALL
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)
```

The final proof is the profile-specific `LISTDSD` output for each profile. The final `SEARCH` command did not list the created profiles in the captured evidence, so it is documented as a verification quirk to revisit in a later lab.
