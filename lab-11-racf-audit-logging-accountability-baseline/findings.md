# Findings

## 1. SETROPTS LIST shows RACF classes and global options

The captured `SETROPTS LIST` output shows RACF active classes including `DATASET`, `FACILITY`, `SERVAUTH`, `SERVER`, `STARTED`, `SURROGAT`, `TSOPROC`, `UNIXMAP`, `UNIXPRIV`, `VMBATCH`, `VMMDISK`, `VMNODE`, `VMRDR`, `VSEMEMT`, `VTAMAPPL`, `VTAMBR`, `WRITER`, `XCSFKEY`, and `XFACILIT`.

The RACLIST section shows `STARTED` among the SETR RACLIST classes. This matters because changes to STARTED profiles normally require RACLIST refresh to take effect.

The global options captured include:

```text
GLOBAL=YES RACLIST ONLY = NONE
AUTOMATIC DATASET PROTECTION IS NOT IN EFFECT
ENHANCED GENERIC NAMING IS NOT IN EFFECT
PROTECT-ALL OPTION IS NOT IN EFFECT
ERASE-ON-SCRATCH IS INACTIVE
LIST OF GROUPS ACCESS CHECKING IS ACTIVE
NO DATA SET MODELLING BEING DONE
PASSWORD CHANGE INTERVAL IS 180 DAYS
MIXED CASE PASSWORD SUPPORT IS NOT IN EFFECT
NO PASSWORD HISTORY BEING MAINTAINED
SECURITY LABEL BY SYSTEM IS NOT IN EFFECT
```

### Interpretation

The environment has RACF active and a broad set of classes available, but several hardening-oriented options are not enabled. This is consistent with an ADCD/training system rather than a production hardened baseline.

## 2. No profiles in WARNING found for tested classes

The following commands returned `ICH31005I NO ENTRIES MEET SEARCH CRITERIA`:

```text
SEARCH CLASS(DATASET) WARNING
SEARCH CLASS(FACILITY) WARNING
SEARCH CLASS(STARTED) WARNING
SEARCH CLASS(SERVAUTH) WARNING
```

### Interpretation

No WARNING profiles were found in the tested classes. This is positive in the narrow sense that the evidence did not show profiles that only warn instead of enforcing. It does not prove the environment is secure, because previous labs found many cases where profiles are simply absent.

## 3. AUDIT search syntax was not accepted as expected

The command output shows:

```text
IKJ56712I INVALID KEYWORD, AUDIT
IKJ56703A REENTER THIS OPERAND -
```

This was seen when testing `SEARCH CLASS(DATASET) AUDIT` and `SEARCH CLASS(SERVAUTH) AUDIT`.

### Interpretation

The intended high-level question was valid - find explicitly audited profiles - but this RACF/TSO command syntax did not accept `AUDIT` as used. The lab therefore relies on direct `RLIST ... ALL` review for the profiles known to be relevant.

## 4. STARTED TCPIP.* has UACC(NONE), WARNING(NO), and audit failures

`RLIST STARTED TCPIP.* ALL` shows:

```text
CLASS      STARTED
NAME       TCPIP.* (G)
OWNER      IBMUSER
UACC       NONE
YOUR ACCESS ALTER
WARNING    NO
AUDITING   FAILURES(READ)
USER       IBMUSER ALTER
```

### Interpretation

This profile is not open by UACC and is not in WARNING mode. It audits failures at READ level. That is useful baseline accountability, but it does not audit all successful sensitive use.

## 5. STARTED FTPD.* has UACC(NONE), WARNING(NO), and audit failures

`RLIST STARTED FTPD.* ALL` shows:

```text
CLASS      STARTED
NAME       FTPD.* (G)
OWNER      IBMUSER
UACC       NONE
YOUR ACCESS ALTER
WARNING    NO
AUDITING   FAILURES(READ)
USER       IBMUSER ALTER
```

### Interpretation

This mirrors TCPIP.*: a defined STARTED profile exists, it is not open, it is not warning-only, and it audits failures at READ level.

## 6. STARTED HZSPROC has UACC(NONE), WARNING(NO), and audit failures

`RLIST STARTED HZSPROC ALL` shows:

```text
CLASS      STARTED
NAME       HZSPROC
OWNER      IBMUSER
UACC       NONE
YOUR ACCESS ALTER
WARNING    NO
AUDITING   FAILURES(READ)
USER       IBMUSER ALTER
```

### Interpretation

The Health Checker started task profile is present and has a similar audit pattern: failures at READ level.

## 7. SMF MAN recording is not being used

`/D SMF` returned:

```text
IEE351I SMF SYS1.MAN RECORDING NOT BEING USED
```

### Interpretation

This is the strongest audit/logging finding in the lab. RACF may make access decisions and some profiles may have AUDITING settings, but this screen indicates that classic SYS1.MAN recording is not being used at the time of capture.

## 8. `/D SMF,O` shows SMFPRM00 parameters

`/D SMF,O` shows:

```text
MEMBER = SMFPRM00
SMFDLEXIT(USER3(IRRADU86)) -- DEFAULT
SMFDLEXIT(USER2(IRRADU00)) -- DEFAULT
SMFDPEXIT(USER3(IRRADU86)) -- DEFAULT
SMFDPEXIT(USER2(IRRADU00)) -- DEFAULT
SUBSYS(STC,NOTYPE(14:19,62:69)) -- SYS
SYS(NOTYPE(14:19,62:69)) -- PARMLIB
```

### Interpretation

The system has SMF parameters available and points to `SMFPRM00`, including exits related to RACF unload/reporting components. However, this must be read together with the `/D SMF` finding and the `NOACTIVE` setting in the member.

## 9. SMFPRM00 shows NOACTIVE and SYS1.MAN datasets

Browse of `ADCD.Z111S.PARMLIB(SMFPRM00)` shows:

```text
NOACTIVE
DSNAME(SYS1.MAN1,
       SYS1.MAN2,
       SYS1.MAN3)
NOPROMPT
REC(PERM)
MAXDORM(3000)
STATUS(010000)
JWT(0400)
SID(SYS1)
LISTDSN
SYS(NOTYPE(14:19,62:69),EXITS(...),NOINTERVAL,NODETAIL)
SUBSYS(STC,EXITS(...))
```

### Interpretation

The parameter member contains SMF dataset definitions and rules, but `NOACTIVE` aligns with the console message that SYS1.MAN recording is not being used.
