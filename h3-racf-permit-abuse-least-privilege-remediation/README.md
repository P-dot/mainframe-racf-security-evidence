# H3 — RACF PERMIT Abuse & Least Privilege Remediation

## Purpose

This lab demonstrates that a RACF DATASET profile with `UACC(NONE)` is not automatically safe. The access list must also follow least privilege.

The lab uses the existing sandbox created in H1/H2:

```text
IBMUSER.SECLAB.*
```

No production datasets, system libraries, started tasks, DB2 objects, CICS objects, APF libraries, or `SYS1.*` / `ADCD.Z111S.*` resources were modified.

## Security idea

A common RACF review mistake is to stop at the `UACC` field. In this lab the protected profile is closed by default, but an explicit `PERMIT` can still grant excessive authority.

```text
UACC(NONE) + USER2 READ   = controlled minimum access
UACC(NONE) + USER2 ALTER  = excessive access for a read-only use case
UACC(NONE) + USER2 READ   = remediated state
```

## Scope

- RACF class: `DATASET`
- Sandbox profile: `IBMUSER.SECLAB.GRANTED.*`
- Test identity: `USER2`
- Control tested: `PERMIT`
- Final state: `USER2 READ`

## Evidence summary

The evidence screenshots show:

1. The `GRANTED` profile has `UNIVERSAL ACCESS: NONE`.
2. `USER2` starts with `READ` access.
3. The command history captures a temporary `PERMIT ... ACCESS(ALTER)` action.
4. The profile is remediated back to `USER2 READ`.
5. The final comparison confirms the intended sandbox model:
   - `PUBLIC` remains open with `UACC(READ)` for the H2 exposure scenario.
   - `PRIVATE` remains closed with `UACC(NONE)`.
   - `GRANTED` remains closed with explicit `USER2 READ` access.

## Evidence limitation

The screenshots clearly capture the baseline and final `USER2 READ` state. The temporary elevation to `USER2 ALTER` is visible in the command history, but the exact access-list line showing `USER2 ALTER` was not captured before remediation. This is documented honestly as a learning point: when proving a before/after security state, capture the access-list line immediately after each change.

## Main finding

A RACF profile must be reviewed as a full decision object:

```text
PROFILE NAME
OWNER
UACC
WARNING
AUDIT
ACCESS LIST
CONDITIONAL ACCESS LIST
```

A profile can have `UACC(NONE)` and still be risky if the access list grants unnecessary `ALTER`, `CONTROL`, or `UPDATE` permissions.

## Final conclusion

This lab moves from simple UACC review into real RACF access-list analysis. The professional skill is not only creating a profile, but detecting and correcting excessive explicit permissions without touching critical system resources.
