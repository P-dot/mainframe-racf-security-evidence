# Findings

## F29-01 — UNIXPRIV control framework is enabled
Severity: Informational / Positive control

UNIXPRIV is ACTIVE, GENERIC and RACLISTed.

## F29-02 — UNIXPRIV filesystem profiles use closed default access
Severity: Low / Positive control

The four observed profiles use UACC(NONE) and WARNING(NO).

## F29-03 — IBMUSER holds explicit authority across all observed UNIXPRIV profiles
Severity: Medium in a production design; contextual in ADCD

The administrative identity is explicitly present with ALTER on each observed profile. This demonstrates concentration of privileged authorization.

## F29-04 — Multiple identities are represented in UNIXMAP U0
Severity: High review priority

The UID(0) mapping contains multiple technical and administrative identities. This is not a recommendation to change them immediately. Each service dependency must be understood first.

## F29-05 — Native UID search unavailable with current identity-mapping implementation
Severity: Informational

The attempted UID=0 USER SEARCH returned ICH31028I. The lab preserved the failure as evidence and used RLIST UNIXMAP U0 ALL instead.

## Recommended next action

Use the already established non-privileged `H7USER` test identity in Lab 30 for controlled UNIXPRIV delegation testing. Do not experiment on critical UID(0) service identities.
