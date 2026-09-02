# Findings — Lab 30

## F-30-01 — H7USER required a valid UNIX security identity
H7USER was suitable as a controlled RACF test identity but initially lacked a usable OMVS segment. H7GRP also lacked OMVS/GID information. A nonzero UID/GID pair was introduced so the account could participate in z/OS UNIX tests without UID(0).

## F-30-02 — RACF HOME definition does not provision the filesystem
Defining `HOME('/u/h7user')` did not create `/u/h7user`. The initial OMVS startup therefore failed with `FSUM2384` until the directory was physically created.

## F-30-03 — Baseline access correctly denied the privileged operation
With no explicit access to `SUPERUSER.FILESYS.CHANGEPERMS`, H7USER could not change the mode of the controlled IBMUSER-owned file.

## F-30-04 — Granular UNIXPRIV delegation was effective
After granting H7USER `READ` to the specific UNIXPRIV profile and refreshing the RACLISTed class, the same operation succeeded.

## F-30-05 — Rollback was effective, not merely cosmetic
After deleting H7USER from the profile ACL and refreshing UNIXPRIV, the functional operation was denied again. This validates both configuration rollback and effective authorization rollback.

## F-30-06 — Least privilege is preferable to UID(0)
The test capability was achieved without assigning UID(0), `SPECIAL`, or `OPERATIONS`. The lab demonstrates a narrower SAF/RACF control path for a specific z/OS UNIX privileged function.
