# Hardening notes

For production-style RACF administration:

- Keep sensitive profiles at `UACC(NONE)`.
- Prefer explicit access-list entries over broad universal access.
- Grant the minimum access level required.
- Use groups for recurring business roles, and user-level permits only for controlled exceptions.
- Remove temporary permissions after the approved window.
- Validate both the successful exception and the final denied state.
- Correlate access denials with SYSLOG/ICH408I and, where available, SMF/RACF reporting.
