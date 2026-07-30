# Commands — H3 RACF PERMIT Abuse & Least Privilege Remediation

## 1. Baseline

Run from ISPF option 6:

```text
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```

Expected baseline:

```text
UNIVERSAL ACCESS: NONE
USER2 READ
```

## 2. Simulate excessive access

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(ALTER)
```

Then verify:

```text
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```

Target observation:

```text
USER2 ALTER
```

## 3. Remediate to least privilege

```text
PERMIT 'IBMUSER.SECLAB.GRANTED.*' CLASS(DATASET) ID(USER2) ACCESS(READ)
```

Then verify:

```text
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
```

Expected remediated state:

```text
USER2 READ
```

## 4. Refresh generic DATASET profiles

```text
SETROPTS GENERIC(DATASET) REFRESH
```

## 5. Final comparison

```text
LISTDSD DA('IBMUSER.SECLAB.PUBLIC.*') ALL
LISTDSD DA('IBMUSER.SECLAB.PRIVATE.*') ALL
LISTDSD DA('IBMUSER.SECLAB.GRANTED.*') ALL
SEARCH CLASS(DATASET) MASK(IBMUSER.SECLAB*)
```

## Commands not used

The lab deliberately avoids:

```text
DELDSD
DELETE
ALTUSER USER2
ADDUSER
PERMIT over SYS1.*
PERMIT over ADCD.Z111S.*
DB2 ARCHIVE LOG
APF / SETPROG changes
STARTED class changes
```
