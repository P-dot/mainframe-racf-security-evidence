# SMF and Accountability Review

## Console display

Command:

```text
/D SMF
```

Observed message:

```text
IEE351I SMF SYS1.MAN RECORDING NOT BEING USED
```

This means the lab cannot claim that classic SYS1.MAN recording is actively collecting events from this evidence alone.

## SMF options display

Command:

```text
/D SMF,O
```

Observed:

```text
MEMBER = SMFPRM00
SUBSYS(STC,NOTYPE(14:19,62:69))
SYS(NOTYPE(14:19,62:69))
```

The system displays SMF parameters from SMFPRM00, but operational recording must be interpreted with the `/D SMF` result.

## SMFPRM00 browse

Dataset/member:

```text
ADCD.Z111S.PARMLIB(SMFPRM00)
```

Key observed lines:

```text
NOACTIVE
DSNAME(SYS1.MAN1,
       SYS1.MAN2,
       SYS1.MAN3)
```

The presence of `DSNAME(SYS1.MAN1, SYS1.MAN2, SYS1.MAN3)` shows configured MAN datasets. The `NOACTIVE` statement explains why the active recording status is weak in this lab.
