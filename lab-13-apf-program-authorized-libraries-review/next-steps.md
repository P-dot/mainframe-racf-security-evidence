# Next Steps

The lab has enough evidence to close. Optional follow-up checks after the system is stable:

```text
LISTDSD DA('SYS1.LINKLIB') ALL
LISTDSD DA('CBC.SCLBDLL') ALL
SEARCH CLASS(DATASET) MASK(CBC.*)
SEARCH CLASS(DATASET) MASK(CEE.*)
SEARCH CLASS(DATASET) MASK(ADCD.Z111S.*)
SEARCH CLASS(OPERCMDS) MASK(MVS.SETPROG*)
SEARCH CLASS(OPERCMDS) MASK(MVS.DISPLAY.PROG*)
```

These are not required to close Lab 13. They would only improve the depth of the APF/PROGRAM authorized-code review.

No hardening commands should be executed until the baseline audit series is complete and a recovery plan exists.
