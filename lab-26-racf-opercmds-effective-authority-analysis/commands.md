# Lab 26 — Commands

## RACF / TSO inquiry

```text
SETROPTS LIST
SEARCH CLASS(OPERCMDS)
RLIST OPERCMDS * ALL
LU IBMUSER
LU IBMUSER NORACF
RLIST OPERCMDS MVS.SETPROG ALL
RLIST OPERCMDS MVS.SETPROG AUTHUSER
RLIST OPERCMDS MVS.SET.PROG ALL
RLIST OPERCMDS MVS.** ALL
```

## SDSF / MVS command validation

```text
/D A,L
/D IPLINFO
/D OPDATA
/SETPROG APF,DISPLAY
WHO
```

## Troubleshooting evidence

`LU IBMUSER NORACF` returned:

```text
ICH300I NO USERS LISTED. NORACF SPECIFIED AND NO OTHER SEGMENTS REQUESTED.
```

This was retained as troubleshooting evidence and did not prevent completion.

`/SETPROG APF,DISPLAY` returned:

```text
IEE345I SETPROG AUTHORITY INVALID, FAILED BY SECURITY
```

This was retained as positive evidence of effective security enforcement.
