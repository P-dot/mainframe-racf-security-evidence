# Production Recommendations

## Identity and privilege

- Minimize `SPECIAL`, `OPERATIONS`, `AUDITOR` and UID(0).
- Use protected IDs for started tasks.
- Avoid shared UNIX UIDs except where technically justified and documented.
- Ensure every technical ID has an owner and purpose.

## DATASET protection

- Protect all system libraries, product libraries, APF libraries, PARMLIB, PROCLIB, zFS aggregates and operational datasets.
- Use generic profiles carefully.
- Use `UACC(NONE)` for sensitive resources.
- Permit access through groups, not individual users where possible.

## OMVS / zFS

- Map every mounted filesystem to a backing dataset.
- Protect backing datasets with RACF DATASET profiles.
- Review mount options and UNIX permissions.
- Reduce UID(0) and prefer granular privileges.

## SAF general resources

- Implement controls for `FACILITY`, `UNIXPRIV`, `SERVAUTH`, `OPERCMDS`, `JESJOBS`, `JESSPOOL` and `SDSF` where relevant.
- Use RACLIST and refresh procedures carefully.
- Document every profile purpose and owner.

## Audit and monitoring

- Validate SMF recording.
- Capture RACF events needed for incident response.
- Audit privileged access, update access to sensitive libraries, and denied access attempts.
- Keep retention aligned with investigation requirements.

## Authorized code

- Protect APF and LINKLIST libraries before enabling or relying on PROGRAM control.
- Control dynamic APF changes through `OPERCMDS`.
- Audit all update access to authorized libraries.

## Change management

- Never make broad RACF changes without backup and rollback.
- Test changes first on non-critical resources.
- Use peer review for production RACF changes.
