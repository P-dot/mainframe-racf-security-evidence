# Evidence Walkthrough

## 01–02 — User and group baseline

The evidence shows `H7USER` as a test identity with default group `H7GRP`. A command attempt using `LISTUSER H7GRP` appears in the evidence and returns an expected error because `H7GRP` is a group, not a user. This is documented as command correction context.

## 03–06 — GROUP.DATA creation and content

`IBMUSER.SECLAB.GROUP.DATA` is cataloged on `SBSYS1`, then populated with benign test text explaining that the dataset is protected by a group-based access model.

## 07–10 — RACF profile creation

`IBMUSER.SECLAB.GROUP.*` is created with `UACC(NONE)` and `AUDIT(FAILURES(READ))`. Initial profile evidence shows no standard access list entries.

## 11–14 — Group READ access

`H7GRP` is granted `READ` using `PERMIT`. The following `LISTDSD` evidence shows `H7GRP READ` in the access list.

## 15–18 — Group access removal

The `PERMIT ... ID(H7GRP) DELETE` command removes the group access entry. Final `LISTDSD` evidence shows no standard access list entries.

## 19–22 — SYSLOG/RACF violation context

The final screenshots include ICH408I/H7USER evidence in SDSF. Some messages refer to `PRIVATE.DATA`, not `GROUP.DATA`, and are therefore retained as context rather than claimed as direct proof of the group dataset test.

