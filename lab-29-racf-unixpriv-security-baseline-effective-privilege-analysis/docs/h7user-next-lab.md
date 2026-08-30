# H7USER reuse decision

The repository already contains `h7-racf-test-identity-lab`.

That lab defines H7USER as a controlled, non-privileged RACF test identity intended for later allow/deny security testing without using IBMUSER as the actor.

Repository-established properties include:
- default group H7GRP
- working TSO/ISPF access
- no visible global privileged attributes in the captured H7 evidence
- no UID(0), STARTED, APF, OPERCMDS, SPECIAL, OPERATIONS, or AUDITOR authority granted by that lab

Decision for the UNIXPRIV sequence:
- Lab 29: do not modify H7USER; baseline only.
- Lab 30: prefer H7USER as the controlled actor for narrowly scoped UNIXPRIV delegation and rollback validation.
- Do not use critical technical UID(0) identities as experimental subjects.
