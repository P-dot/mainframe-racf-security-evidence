# Findings — Lab 32

## Finding 1 — RACDCERT authority is function-specific

The controlled test demonstrated distinct authorization boundaries for LISTRING, ADDRING, and DELRING. Authority for one operation did not implicitly authorize the others.

## Finding 2 — Least privilege was sufficient

The lab did not require SPECIAL, OPERATIONS, UID(0), or broad cryptographic administration. Narrow FACILITY access was sufficient for the selected controlled operations.

## Finding 3 — Successful ADDRING was verified by state

Because ADDRING completed without a positive success message, the result was independently verified with `LISTRING(*)`, which displayed `LAB32RING` and confirmed that no certificates were connected.

## Finding 4 — Deletion required its own authorization path

`DELRING(LAB32RING)` was denied after successful ring creation. A separate DELRING control and delegation were required before deletion succeeded.

## Finding 5 — RACLIST refresh affects effective authority

Removing an ACL entry from the RACF database did not immediately remove effective authorization from the RACLISTed FACILITY class. The same LISTRING operation remained usable until `SETROPTS RACLIST(FACILITY) REFRESH` was issued.

This was demonstrated functionally, not inferred from configuration alone.

## Finding 6 — Full rollback was achieved

The synthetic ring was removed, H7USER's temporary permissions were removed, the original denial was restored, and both temporary FACILITY profiles were deleted and verified absent.

## Security conclusion

Lab 32 demonstrates a controlled administrative privilege lifecycle for RACF cryptographic objects while maintaining a non-privileged test subject and restoring the pre-lab state.
