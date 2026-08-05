# Hardening Notes

A safe RACF test identity should be:

- owned by an administrative lab owner
- connected only to a lab group
- created without privileged attributes
- granted only the minimum dataset permissions required for the test
- given TSO access only when interactive testing is required
- revoked or removed after the training series if no longer needed

This lab creates the test identity but does not grant production permissions.
