# H10 — RACF Temporary Access Remediation Lab

This lab demonstrates a controlled RACF exception lifecycle using a non-privileged test identity.

The scenario starts from a denied access to `IBMUSER.SECLAB.PRIVATE.DATA`, grants temporary READ access to `H7USER`, verifies successful access, removes the exception, and confirms that access is denied again.

## Lab scope

- RACF DATASET class only.
- Sandbox resources under `IBMUSER.SECLAB.*`.
- Test identity: `H7USER`.
- No production datasets modified.
- No changes to `SYS1.*`, `ADCD.Z111S.*`, APF, STARTED, DB2, CICS, or OPERCMDS.

## Main result

`H7USER` was denied access to `PRIVATE.DATA`, then granted temporary READ through a profile access-list entry, then the permission was removed and the original denial was restored.

## Evidence-driven value

This is a practical model for temporary access management in RACF: grant narrowly, verify, remove, and verify the denied state again.
