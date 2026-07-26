# APF and LINKLIST Review

## APF display

Screenshot: `evidence/screenshots/02_display_prog_apf_active_list.png`

Command:

```text
/D PROG,APF
```

Visible APF-authorized libraries include examples such as:

```text
SYS1.LINKLIB
SYS1.SVCLIB
SYS1.MIGLIB
SYS1.CSSLIB
SYS1.SIEALNKE
CSQ700.*
TCPIP.*
USER.VTAMLIB
```

## LINKLIST display

Screenshot: `evidence/screenshots/03_display_prog_lnklst_active_list.png`

Command:

```text
/D PROG,LNKLST
```

Visible LINKLIST libraries include examples such as:

```text
SYS1.LINKLIB
SYS1.MIGLIB
SYS1.CSSLIB
SYS1.SIEALNKE
SYS1.SERBLINK
CEE.SCEERUN
CEE.SCEERUN2
```

## LPA display attempt

Screenshot: `evidence/screenshots/04_display_prog_lpa_syntax_error.png`

The LPA display attempt returned a syntax error. No LPA inventory is therefore claimed from the captured Lab 13 evidence.

## Finding

```text
The system has active APF-authorized and LINKLIST library structures. Several core system, middleware, TCP/IP, and runtime libraries appear in the captured displays.
```
