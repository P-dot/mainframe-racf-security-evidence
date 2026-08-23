# Findings — Lab 28

## F-01 — Native RACF panels provide a complete introductory administration path

**Observation:** Group creation, user connection, DATASET profile creation, access-list maintenance and system-option review were completed through RACF ISPF services.

**Assessment:** PASS. The lab demonstrates RACF administration beyond command-only TSO usage.

## F-02 — Group connection kept intentionally limited

**Observation:** `IBMUSER` was connected to `RACFL28` with `USE`, `DEFAULT UACC=NONE`, and no group-SPECIAL, group-OPERATIONS or group-AUDITOR attributes.

**Assessment:** PASS. The connection itself was not unnecessarily elevated.

## F-03 — Generic DATASET profile uses deny-by-default posture

**Observation:** The created generic profile uses `UACC(NONE)`, normal enforcement (`WARNING=NO`) and failure auditing from READ.

**Assessment:** PASS.

## F-04 — Access granted to the group, not directly to the user

**Observation:** The standard access list contains `RACFL28 ALTER`.

**Assessment:** PASS. This demonstrates group-based resource authorization.

## F-05 — EGN is not active

**Observation:** `RACFL28.**` was rejected. System-option display showed `ENHANCED GENERIC NAMING IS NOT IN EFFECT`.

**Assessment:** INFORMATIONAL. The lab adapted to the existing configuration and did not introduce an unnecessary global RACF change.

## F-06 — Functional test has a privileged-identity limitation

**Observation:** `IBMUSER.RACFL28.TEST` was allocated, written and read successfully.

**Assessment:** PASS for functional namespace validation, but not a conclusive least-privilege proof because `IBMUSER` is privileged.

**Future enhancement:** repeat the access test with a dedicated non-privileged identity if a later lab specifically targets effective-access proof.
