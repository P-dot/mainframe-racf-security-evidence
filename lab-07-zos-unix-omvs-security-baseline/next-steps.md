# Next Steps

## Immediate follow-up commands

The current lab is valid, but the following commands would improve evidence quality:

```text
LISTUSER START2 OMVS
LISTGRP IMWEB
LISTGRP SYS1 OMVS
LISTGRP OMVSGRP OMVS
RLIST UNIXMAP G* ALL
SEARCH CLASS(GROUP) GID(0)
```

Some of these might fail depending on AIM level and RACF configuration. Capture the errors as evidence.

## Suggested next lab

```text
Lab 08 — RACF Dataset Protection Baseline for z/OS UNIX and System Libraries
```

Candidate objects:

```text
BPXPRMxx
OMVS parmlib members
zFS datasets
ADCD.Z111S.LINKLIB
ADCD.Z111S.PARMLIB
SYS1.PARMLIB
SYS1.LINKLIB
```

The next lab should connect z/OS UNIX identity risk with data set protection risk.
