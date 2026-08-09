# Attack Scenario — Excessive Direct User Permissions

The unsafe pattern is to grant dataset access directly to every user:

```text
PERMIT profile ID(USERA) ACCESS(READ)
PERMIT profile ID(USERB) ACCESS(READ)
PERMIT profile ID(USERC) ACCESS(READ)
```

This becomes hard to govern. When users transfer, change teams, or leave, the access list becomes cluttered and risky.

The controlled scenario in this lab shows the better model:

```text
PERMIT 'IBMUSER.SECLAB.GROUP.*' CLASS(DATASET) ID(H7GRP) ACCESS(READ)
```

The user does not need to be present in the dataset profile access list. The access path is through group membership.

