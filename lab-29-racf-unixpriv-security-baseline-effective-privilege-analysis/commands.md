# Commands and panel actions

## 1. RACF system options display

Purpose: establish whether `UNIXPRIV` is active, supports generic profiles, and is RACLISTed.

Path:

RACF Services → System Options → Display

Result:
- UNIXPRIV ACTIVE
- UNIXPRIV GENERIC
- UNIXPRIV RACLIST

This is inquiry-only and does not alter SETROPTS.

## 2. General-resource inventory

Path:

RACF Services → General Resource Profiles → SEARCH

Class:

UNIXPRIV

No mask/filter was applied because the objective was a complete class inventory.

Result:
- SUPERUSER.FILESYS
- SUPERUSER.FILESYS.CHANGEPERMS
- SUPERUSER.FILESYS.CHOWN
- SUPERUSER.FILESYS.MOUNT

## 3. Profile display and access-list review

Each profile was displayed as a generic profile with ACCESS LIST selected.

Result for all four:
- UACC: NONE
- WARNING: NO
- AUDITING: FAILURES(READ)
- IBMUSER: ALTER
- no conditional access entries observed

Purpose: distinguish profile existence from effective authorization.

## 4. UID(0) search attempt

RACF Services → User Profiles → SEARCH

Criterion:

UID = 0

Result:

ICH31028I THE UID KEYWORD REQUIRES APPLICATION IDENTITY MAPPING TO BE IMPLEMENTED.

Meaning: this installation cannot execute the requested UID-keyword search using the current identity-mapping implementation. No configuration was changed.

## 5. Compatible read-only fallback

Command:

RLIST UNIXMAP U0 ALL

What it is:
- `RLIST`: displays a RACF general-resource profile.
- `UNIXMAP`: RACF mapping class used by this environment.
- `U0`: mapping associated with UNIX UID 0.
- `ALL`: requests full profile information.

Why it was used:
It provides a compatible read-only way to inspect the UID(0) mapping after the USER SEARCH method was blocked by `ICH31028I`.

Result:
The U0 profile exists and contains multiple technical/administrative identities.

## Change control

No change command was executed in Lab 29.

Commands intentionally NOT used:
- RDEFINE
- RALTER
- PERMIT
- ALTUSER
- SETROPTS CLASSACT(...)
- SETROPTS RACLIST(...) REFRESH

Those belong to a later controlled-change lab.
