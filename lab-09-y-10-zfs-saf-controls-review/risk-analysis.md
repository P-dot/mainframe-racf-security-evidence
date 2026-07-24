# Risk analysis

## Overall risk pattern

The combined Lab 09/10 documents this pattern:

```text
Active zFS RDWR
+
zFS backing datasets without visible RACF DATASET profiles
+
no visible BPX.* / UNIXPRIV / checked EZB.* profiles
+
prior evidence of several UID(0) technical users
```

For ADCD this is useful as a training baseline. For production it would represent a hardening gap.

## Main risks

### 1. Weak dataset-level accountability

If backing datasets are not covered by RACF DATASET profiles, access control and auditing may depend on defaults, catalog protections, or environmental assumptions rather than explicit least privilege profiles.

### 2. Overdependence on UID(0)

Earlier labs showed many technical users mapped to UID(0). Without granular `UNIXPRIV` or `BPX.*` controls, the system model appears to rely more on broad UNIX superuser authority than on fine-grained SAF controls.

### 3. Missing or unverified TCP/IP granular controls

No checked `EZB.*` SERVAUTH profiles were found. In a hardened environment, controls such as NETSTAT access, stack access, protected port access, and Policy Agent authority should be explicitly reviewed.

### 4. STARTED profile not enough without STDATA visibility

`TCPIP.*` exists as a STARTED profile, but the evidence did not show `STDATA`. A profile can exist while runtime identity assignment is handled elsewhere or not visible in the captured output.

## Production hardening direction

Do not apply these changes directly to ADCD without a recovery plan. For a production-style design, the direction would be:

```text
Define DATASET profiles for zFS backing datasets
Set UACC(NONE) for sensitive zFS and system libraries
Use explicit access lists for technical IDs/groups
Implement granular UNIXPRIV where appropriate
Define/review BPX.* FACILITY profiles
Define/review EZB.* SERVAUTH profiles
Audit failures and selected successes for sensitive resources
Validate with Health Checker, DSMON or zSecure where available
```
