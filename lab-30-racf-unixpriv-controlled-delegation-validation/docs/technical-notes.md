# Technical Notes — Lab 30

## Why two sessions were used
IBMUSER remained the administrative actor. H7USER remained the subject under test. Separating the two roles prevents an administrator's authority from contaminating the functional test result.

## Why UID(0) was avoided
The purpose of the lab is to demonstrate granular delegation. UID(0) would bypass the core learning objective by granting broad UNIX superuser semantics.

## Why RACLIST refresh matters
UNIXPRIV was already RACLISTed in the system baseline. After changing an ACL, `SETROPTS RACLIST(UNIXPRIV) REFRESH` was used so the in-storage profiles reflected the RACF database change before the functional test.

## Why the same operation was repeated
A before/after test is strongest when the actor, target, and requested action remain stable. This reduces alternative explanations for the observed change in behavior.
