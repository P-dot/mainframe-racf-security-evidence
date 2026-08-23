# Findings

## F27-01 — Generic warning mode removed
**RESOLVED WITHIN TESTED SCOPE.** `MVS.**` changed from `WARNING YES` to `WARNING NO`; `UACC(NONE)` and documented `IBMUSER READ` remained.

## F27-02 — RACLIST refresh requirement confirmed
After RALTER, RACF explicitly stated that RACLISTed OPERCMDS profiles would not reflect updates until SETROPTS REFRESH. The refresh was performed and the resulting profile state verified.

## F27-03 — Selected legitimate queries preserved
`D A,L`, `D IPLINFO`, and `D OPDATA` succeeded both before and after hardening.

## F27-04 — Existing security denial preserved
`SETPROG APF,DISPLAY` returned `IEE345I ... FAILED BY SECURITY` both before and after.

## Limitation
No conclusion is made about untested MVS operator commands.
