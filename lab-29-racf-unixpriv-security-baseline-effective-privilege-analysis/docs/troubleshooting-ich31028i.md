# Troubleshooting — ICH31028I

## Attempt

RACF USER PROFILE SEARCH with:

UID = 0

## Result

ICH31028I THE UID KEYWORD REQUIRES APPLICATION IDENTITY MAPPING TO BE IMPLEMENTED.

## Interpretation

The panel accepts the UID search criterion, but the installation cannot execute that search with its current identity-mapping implementation.

## Decision

Do not change global RACF configuration simply to complete a baseline query.

## Alternative used

RLIST UNIXMAP U0 ALL

This returned the existing U0 mapping and allowed the UID(0) exposure baseline to continue read-only.
