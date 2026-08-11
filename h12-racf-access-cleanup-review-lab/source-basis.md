# Source Basis

This lab is based on the RACF administration pattern already built in the H-series labs:

- H7 created the non-privileged test identity.
- H8 proved real access behavior with `H7USER`.
- H9 correlated denials with `ICH408I`.
- H10 demonstrated temporary access and removal.
- H11 demonstrated group-based access.
- H12 cleans obsolete access-list entries without deleting profiles.

Conceptual basis:

- RACF profiles protect resources.
- `UACC` controls universal access.
- The access list controls explicit users and groups.
- `PERMIT ... DELETE` removes an access-list entry.
- `SETROPTS GENERIC(DATASET) REFRESH` refreshes generic DATASET profile processing.
