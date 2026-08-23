# Evidence Index

The screenshots were extracted from the final 8-page Lab 27 evidence document supplied for closure.

They document the initial `WARNING YES` baseline, SETROPTS baseline, BEFORE command tests, `RALTER ... NOWARNING`, RACF's RACLIST refresh warning, `SETROPTS RACLIST(OPERCMDS) REFRESH`, post-change `WARNING NO`, and the complete AFTER test set.

Key final evidence: the selected DISPLAY commands still return valid output while `SETPROG APF,DISPLAY` remains rejected with `IEE345I ... FAILED BY SECURITY`.
