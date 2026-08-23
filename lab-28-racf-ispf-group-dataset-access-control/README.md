# Lab 28 — RACF ISPF Group, Dataset Profile & Group-Based Access Control

**Status:** COMPLETED — CONFIGURATION AND FUNCTIONAL VALIDATION

## Objective

Build a compact RACF administration workflow from the native RACF ISPF panels instead of relying only on TSO option 6 commands.

The lab demonstrates:

- RACF group-profile creation and hierarchy.
- User-to-group connection with limited group authority.
- Generic DATASET profile discovery and creation.
- UACC, warning/enforcement and auditing choices.
- Standard access-list administration.
- Group-based authorization to a protected dataset namespace.
- Troubleshooting of generic naming when Enhanced Generic Naming (EGN) is not active.
- Functional creation, write and read of a dataset under the protected namespace.

## Environment

IBM z/OS ADCD 1.11 · RACF/SAF · TSO/ISPF · RACF ISPF panels · Hercules.

> Scope note: controlled training environment, not production.

## Workflow

### 1. Group profile baseline and creation

A display/search first confirmed that the planned lab group did not already exist.

The group was then created as:

```text
GROUP:     RACFL28
OWNER:     IBMUSER
SUPGROUP:  SYS1
```

An initial invalid-superior-group condition was corrected by using the existing `SYS1` hierarchy.

Verification showed:

```text
SUPERIOR GROUP=SYS1
OWNER=IBMUSER
NO USERS
```

### 2. User-to-group connection

`IBMUSER` was connected to `RACFL28` using deliberately limited connection settings:

```text
DEFAULT UACC:     NONE
GROUP AUTHORITY:  USE
GROUP-SPECIAL:    NO
GROUP-OPERATIONS: NO
GROUP-AUDITOR:    NO
```

A subsequent display confirmed:

```text
IBMUSER  USE
CONNECT ATTRIBUTES=NONE
```

This demonstrates the distinction between group membership/authority and dataset access authority.

### 3. DATASET profile discovery

The RACF DATA SET PROFILE SERVICES panels were used to search for an existing generic profile under the new lab namespace.

The search returned:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

This provided the BEFORE state.

### 4. Generic naming troubleshooting

The first design attempted:

```text
RACFL28.**
```

RACF rejected it with:

```text
IKJ56702I INVALID DATASET NAME
```

The failure was investigated through RACF System Security Options / DISPLAY rather than by repeatedly changing syntax.

The system showed:

```text
GENERIC PROFILE CLASSES = DATASET ...
ENHANCED GENERIC NAMING IS NOT IN EFFECT
```

The lab therefore did **not** enable EGN globally. Instead, it adapted the profile to the system's existing configuration and used:

```text
RACFL28.*
TYPE=GENERIC
```

This was accepted with `PROFILE ADDED`.

### 5. DATASET profile security policy

The generic profile was created with:

```text
OWNER:             RACFL28
UACC:              NONE
FAILED ACCESSES:   FAIL
AUDIT SUCCESSES:   NOAUDIT
AUDIT FAILURES:    READ
WARNING:           NO
```

The objective was a deny-by-default profile with explicit access and useful failure auditing.

### 6. Group-based standard access list

Before the change, the display showed no entries in the standard access list.

The access list was then updated to grant the lab group authority over its lab namespace:

```text
ID       ACCESS
RACFL28  ALTER
```

Verification confirmed the entry while `UACC` remained `NONE`.

### 7. Functional validation

The RACF panel displayed the effective generic profile as:

```text
IBMUSER.RACFL28.* (G)
OWNER=RACFL28
UNIVERSAL ACCESS=NONE
WARNING=NO
AUDITING=FAILURES(READ)

ID       ACCESS
RACFL28  ALTER
```

A sequential test dataset was then allocated under that resolved namespace:

```text
IBMUSER.RACFL28.TEST
```

The dataset was successfully written and read with:

```text
RACF LAB 28 DATASET ACCESS TEST
```

## Security interpretation

The lab demonstrates the RACF authorization chain:

```text
IBMUSER
   |
   | CONNECT / USE
   v
RACFL28
   |
   | standard access list: ALTER
   v
IBMUSER.RACFL28.* (generic DATASET profile)
   |
   v
IBMUSER.RACFL28.TEST
```

`UACC(NONE)` avoids universal access, while the standard access list grants authority explicitly to the group.

## Important limitation

`IBMUSER` is a privileged training identity. Therefore the successful allocation/read/write test proves that the protected namespace and dataset are operational, but it does **not** by itself prove that the effective access came exclusively from the `RACFL28` group entry.

A strict least-privilege authorization proof would require a non-privileged test identity. That extension is intentionally outside this introductory lab.

## Evidence

Original screenshots extracted from the lab evidence document are stored under:

```text
evidence/screenshots/
```

They include the group-profile lifecycle, user connection, DATASET-profile search, EGN troubleshooting, profile creation, access-list BEFORE/AFTER state and final dataset validation.

## Result

**PASS**

The lab completed a native RACF ISPF administration flow from identity/group organization through dataset protection and explicit group authorization, including evidence-driven troubleshooting and functional validation.
