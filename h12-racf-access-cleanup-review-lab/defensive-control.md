# Defensive Control

The defensive control is controlled access-list cleanup.

The correct pattern is:

```text
1. Review the current profile.
2. Identify obsolete access-list entries.
3. Remove only the obsolete entry.
4. Refresh generic DATASET profiles.
5. Re-list the profile.
6. Test that valid access still works.
7. Test that protected data remains denied.
```

The incorrect pattern would be:

```text
DELETE USER2
DELDSD profile
ALTDSD profile UACC(READ)
PERMIT broad group ALTER
```

This lab keeps the profile closed and removes only the unnecessary direct access entry.
