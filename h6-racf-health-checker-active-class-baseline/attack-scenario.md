# Controlled Risk Scenario

This lab does not exploit the system. It models a defensive hardening scenario where RACF classes expected by Health Checker are not fully active.

## Why this matters

If a class such as `UNIXPRIV`, `TEMPDSN`, or `OPERCMDS` is inactive, RACF cannot consistently enforce policy for the corresponding resource area.

The controlled risk is not a single dataset exposure. The risk is a missing enforcement layer:

- `UNIXPRIV`: privileged z/OS UNIX functions
- `TEMPDSN`: temporary dataset control
- `OPERCMDS`: operator command authorization

## OPERCMDS caution

`OPERCMDS` is sensitive because it affects MVS command authorization. For this reason, the lab uses a guarded profile in `WARNING` mode and explicitly permits `IBMUSER` for laboratory continuity.

This is not a recommended production final state. It is a safe transition technique in a lab.
