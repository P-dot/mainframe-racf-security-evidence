# Risk Analysis

## Consolidated pattern

The combined evidence from previous labs and this lab suggests the following ADCD laboratory pattern:

```text
Privileged technical IDs
+ UID(0) exposure in OMVS
+ sensitive datasets/zFS without visible RACF DATASET profiles
+ no matching BPX/UNIXPRIV/SERVAUTH profiles in earlier evidence
+ no matching OPERCMDS/JESJOBS/JESSPOOL/SDSF profiles in this lab
+ successful SDSF display visibility
```

## Risk in a production-like environment

If the same pattern existed in production, the main risks would be:

- Insufficient granular control over operator commands.
- Insufficient granular control over JES job and spool resources.
- Broad visibility over SYSLOG, spool and active address spaces.
- Difficulty proving separation of duties for operations staff.
- Potential reliance on high-authority users instead of SAF profiles.

## ADCD interpretation

For an IBM ADCD training system, this is not surprising. ADCD is designed for learning and lab execution, not as a hardened enterprise production image.

Therefore, the finding should be written as:

```text
Expected laboratory exposure, not a production-ready security baseline.
```

## Production hardening direction

A production review would normally validate and define:

- `OPERCMDS` profiles for MVS/JES2 command families.
- `JESJOBS` profiles for job submission/cancel authority.
- `JESSPOOL` profiles for SYSLOG and job output access.
- `SDSF` SAF profiles for panel/action authority.
- Audit rules for privileged successful and failed activity.
- Separation between read-only display authority and destructive command authority.
