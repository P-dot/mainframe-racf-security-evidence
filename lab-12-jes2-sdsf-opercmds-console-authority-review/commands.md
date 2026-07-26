# Commands Executed

## RACF baseline

```text
SETROPTS LIST
```

Purpose: display RACF global options, active classes, generic profile classes, RACLIST classes and other security settings.

## OPERCMDS searches

```text
SEARCH CLASS(OPERCMDS) MASK(*)
SEARCH CLASS(OPERCMDS) MASK(MVS*)
SEARCH CLASS(OPERCMDS) MASK($*)
SEARCH CLASS(OPERCMDS) MASK(JES2*)
```

Purpose: check whether RACF profiles exist for operator command controls.

## JESJOBS search

```text
SEARCH CLASS(JESJOBS) MASK(*)
```

Purpose: check whether RACF profiles exist for JES job control protection.

## JESSPOOL searches

```text
SEARCH CLASS(JESSPOOL) MASK(*)
SEARCH CLASS(JESSPOOL) MASK(*SYSLOG*)
SEARCH CLASS(JESSPOOL) MASK(*MASTER*)
SEARCH CLASS(JESSPOOL) MASK(*IBMUSER*)
```

Purpose: check whether RACF profiles exist for spool, SYSLOG, MASTER and IBMUSER-related spool resources.

## SDSF profile search

```text
SEARCH CLASS(SDSF) MASK(*)
```

Purpose: check whether RACF SDSF resource profiles are visible in the environment.

## SDSF / console display evidence

```text
/D A,L
/$D SPOOL
```

Purpose: issue read-only display commands from SDSF to test operational visibility and command response.

## Commands intentionally not executed

```text
RDEFINE
RALTER
PERMIT
SETROPTS RACLIST(...) REFRESH
CANCEL
PURGE
STOP
START
MODIFY
```

This lab was evidence-only. No hardening or operationally disruptive command was executed.
