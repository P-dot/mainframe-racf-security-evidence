# Risk analysis

## Risk 1 - Overprivileged administrative account

IBMUSER has SPECIAL and OPERATIONS. This is acceptable for an isolated ADCD learning system, but would be unacceptable as a normal production account.

Potential production control:

- Separate personal IDs from emergency IDs.
- Remove SPECIAL/OPERATIONS from ordinary users.
- Use break-glass IDs with approval and auditing.
- Review users with SPECIAL, OPERATIONS, AUDITOR, ROAUDIT, and CLAUTH.

## Risk 2 - Missing dataset profiles

No RACF profile was found for IBMUSER.* or ADCD.Z111S.LINKLIB.

Potential production control:

- Define dataset profiles with UACC(NONE).
- Grant access explicitly to required users/groups.
- Use WARNING mode only for controlled migration.
- Enable auditing for sensitive libraries.

## Risk 3 - Sensitive library exposure

ADCD.Z111S.LINKLIB is an executable/system-related library. Missing RACF protection is a serious concern because modification of executable libraries can become a system integrity risk.

Potential production control:

- Inventory APF/LINKLIST/LPA/PROCLIB/PARMLIB libraries.
- Protect each with RACF DATASET profiles.
- Review UACC, ID(*), WARNING, and access lists.
- Compare with Health Checker RACF_SENSITIVE_RESOURCES output.

## Risk 4 - PROTECTALL and ADSP inactive

With PROTECTALL and ADSP inactive, datasets can exist without RACF profiles.

Potential production control:

- Evaluate PROTECTALL in a controlled test plan.
- Avoid enabling PROTECTALL blindly on legacy systems.
- Build an inventory and identify applications that rely on unprotected datasets.

## Risk 5 - STARTED task identity control requires review

STARTED class is active and RACLISTed. Profiles exist, but wildcard output is too broad and the current screenshots do not prove all STDATA mappings.

Potential production control:

- Review exact STARTED profiles for critical STCs.
- Confirm STDATA(USER, GROUP, TRUSTED, PRIVILEGED).
- Avoid TRUSTED/PRIVILEGED unless justified.
- Refresh RACLIST after controlled profile changes.
