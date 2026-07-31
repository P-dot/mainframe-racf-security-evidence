# H4 — RACF WARNING Mode Controlled Exposure

## Purpose

This lab demonstrates the risk of RACF `WARNING` mode using a safe dataset profile under `IBMUSER.SECLAB.WARNING.*`.

The lab is intentionally limited to a sandbox namespace. It does not modify `SYS1.*`, `ADCD.Z111S.*`, APF, JES2, DB2, CICS, OPERCMDS, STARTED, or production-like resources.

## Security idea

`UACC(NONE)` normally means the resource is closed by default.  
However, when a profile is in `WARNING` mode, a failed access can be allowed while RACF issues a warning and records the event.

Therefore:

```text
UACC(NONE) + WARNING(YES) != final hardening
```

`WARNING` can be useful during staged rollout, but it must not be left enabled as a permanent control.

## Lab flow

1. Create or validate the `IBMUSER.SECLAB.WARNING.*` sandbox profile.
2. Confirm defensive baseline: `UACC(NONE)` and `WARNING(NO)`.
3. Enable `WARNING` mode.
4. Add audit coverage with `AUDIT(ALL(READ))`.
5. Disable `WARNING` with `NOWARNING`.
6. Refresh generic DATASET profiles.
7. Compare the sandbox state with `PUBLIC`, `PRIVATE`, `GRANTED`, and `WARNING`.

## Final state

```text
IBMUSER.SECLAB.PUBLIC.*   -> UACC(READ)
IBMUSER.SECLAB.PRIVATE.*  -> UACC(NONE)
IBMUSER.SECLAB.GRANTED.*  -> UACC(NONE) + USER2 READ
IBMUSER.SECLAB.WARNING.*  -> UACC(NONE) + WARNING(NO) + AUDIT(ALL(READ))
```

## Evidence

The screenshots are stored in:

```text
evidence/screenshots/
```

A contact sheet is available at:

```text
evidence/h4_contact_sheet.jpg
```
