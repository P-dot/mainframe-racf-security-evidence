# Attack scenario — Controlled temporary exception

A non-privileged user, `H7USER`, attempts to browse `IBMUSER.SECLAB.PRIVATE.DATA`.

The dataset is protected by a RACF profile with `UACC(NONE)`, so the expected baseline is denial.

The lab simulates a real operational request: the user temporarily needs READ access. The risky shortcut would be to change the universal access to `READ`, which would open the resource to every user covered by the profile. This lab avoids that shortcut and uses a narrowly scoped access-list entry instead.
