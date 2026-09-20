# Security Analysis

## Main conclusion

The TN3270/TSO logon panel in the tested environment exposes different response classes depending on whether a controlled identity reaches the password-validation stage.

This matters because externally observable authentication differences can reduce uncertainty during reconnaissance. The practical risk, however, depends on network exposure, monitoring, naming conventions, password controls and other installation-specific defenses.

## What was proven

- TN3270 was listening on TCP port 23 during the test.
- H7USER is a defined controlled RACF identity with usable TSO access.
- USER2 exists in RACF but is not authorized for the tested TSO path.
- H7NOUSR does not exist in RACF.
- USER2 and H7NOUSR produced the same `IKJ56420I` response.
- H7USER with one incorrect password produced a different `IKJ56421I` response.
- H7USER subsequently authenticated successfully.

## What was not proven

- No complete RACF userid-enumeration oracle was demonstrated.
- No password was recovered.
- No password spraying or brute-force attack was performed.
- No account takeover or privilege escalation was demonstrated.
- No claim is made that this behavior is identical on other z/OS releases or installations.

## Hardening perspective

Defensive review can consider:

- limiting TN3270 reachability to intended networks;
- secure TN3270 / TLS migration;
- monitoring repeated authentication failures;
- reviewing TSO-enabled identity population and naming predictability;
- evaluating whether installation-specific controls can reduce externally distinguishable responses;
- ensuring lockout/password policy and incident response are appropriate for the exposure.
