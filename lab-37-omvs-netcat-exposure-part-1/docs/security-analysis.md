# Security Analysis

## Why the blocker matters

A network security experiment must separate:

```text
application behavior
from
transport availability
```

If the host-to-z/OS LCS path is unavailable, failed TCP connections do not prove an application-layer control.

For example:

```text
FTP connection fails
```

does not by itself prove:

```text
FTP authorization denied
```

and:

```text
TN3270 connection fails
```

does not by itself prove:

```text
TN3270 service unavailable
```

when internal `netstat` evidence shows the services listening.

## Security value of Part 1

The lab establishes a trustworthy prerequisite chain before attempting Netcat:

```text
OMVS
 -> socket visibility
 -> compiler readiness
 -> source provenance
 -> network reachability
 -> controlled application test
```

Stopping at the broken network boundary prevents misleading security conclusions.

## No exploitation claim

No remote shell, bind shell, reverse shell, credential interception, privilege escalation, or RACF bypass was demonstrated.

Part 1 is a readiness and fault-isolation lab.
