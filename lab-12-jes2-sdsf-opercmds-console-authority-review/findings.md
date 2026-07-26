# Findings

## Finding 1 — SETROPTS LIST confirms a broad RACF class baseline

The evidence includes multiple screenshots of `SETROPTS LIST`. The class lists show a broad RACF environment with many active and generic command/resource classes visible, including security areas relevant to dataset, started task, JES, SDSF, SERVAUTH and operator command review.

Relevant screenshots:

- `01_SETROPTS_LIST_active_classes.png`
- `02_SETROPTS_LIST_generic_command_classes.png`
- `03_SETROPTS_LIST_global_checking_classes.png`
- `04_SETROPTS_LIST_raclist_and_options.png`
- `05_SETROPTS_LIST_password_and_misc_options.png`

Important observed options include:

```text
PROTECT-ALL OPTION IS NOT IN EFFECT
ERASE-ON-SCRATCH IS INACTIVE
LIST OF GROUPS ACCESS CHECKING IS ACTIVE
PASSWORD CHANGE INTERVAL IS 180 DAYS
MIXED CASE PASSWORD SUPPORT IS NOT IN EFFECT
```

Interpretation: RACF is active and operational, but the baseline is not hardened like a production security baseline.

## Finding 2 — No matching OPERCMDS profiles were found

The following searches returned:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Commands:

```text
SEARCH CLASS(OPERCMDS) MASK(*)
SEARCH CLASS(OPERCMDS) MASK(MVS*)
SEARCH CLASS(OPERCMDS) MASK($*)
SEARCH CLASS(OPERCMDS) MASK(JES2*)
```

Interpretation: the evidence does not show granular RACF `OPERCMDS` profiles for MVS, JES2 or `$` operator commands.

## Finding 3 — No matching JESJOBS profiles were found

The `JESJOBS` search returned no matching entries.

Command:

```text
SEARCH CLASS(JESJOBS) MASK(*)
```

Interpretation: the evidence does not show RACF `JESJOBS` profiles controlling job-related JES authority in this ADCD system.

## Finding 4 — No matching JESSPOOL profiles were found

The following `JESSPOOL` searches returned no matching entries:

```text
SEARCH CLASS(JESSPOOL) MASK(*)
SEARCH CLASS(JESSPOOL) MASK(*SYSLOG*)
SEARCH CLASS(JESSPOOL) MASK(*MASTER*)
SEARCH CLASS(JESSPOOL) MASK(*IBMUSER*)
```

Interpretation: the evidence does not show granular RACF `JESSPOOL` profiles for spool, SYSLOG, MASTER or IBMUSER spool access patterns.

## Finding 5 — No matching SDSF profiles were found

Command:

```text
SEARCH CLASS(SDSF) MASK(*)
```

Result:

```text
ICH31005I NO ENTRIES MEET SEARCH CRITERIA
```

Interpretation: no RACF `SDSF` profiles were found by this broad search.

## Finding 6 — SDSF display command produced active address-space output

The SDSF evidence shows a successful display of active work after command issue. Visible address spaces include examples such as:

```text
JES2
VTAM
RACF
TSO
TCPIP
TN3270
DB9GMSTR
CICSA
HTTPD1
CSQ7MSTR
INETD4
SSHD4
FTPD1
IBMUSER
```

Interpretation: the user session has broad operational visibility in SDSF, at least for display purposes.

## Finding 7 — JES2 spool display returned a normal response

The JES2 display evidence shows:

```text
$HASP893 VOLUME(SBSYS1) STATUS=ACTIVE,PERCENT=52
```

Interpretation: the `$D SPOOL` display command returned JES2 spool status information. This confirms operational visibility into JES2 spool state from the session used in the lab.
