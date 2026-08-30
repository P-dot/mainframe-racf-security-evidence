# Risk analysis

The principal risk is not the existence of UNIXPRIV itself; UNIXPRIV provides a mechanism for granular SAF/RACF control of selected privileged UNIX operations.

The more important exposure is the coexistence of:
1. multiple identities associated with UID(0), and
2. a small set of explicit UNIXPRIV profiles whose observed authorization is concentrated in IBMUSER.

In a production hardening program, UID(0) reduction must be dependency-driven. Started tasks and networking/system identities must not be changed merely because they appear in U0.

Lab 29 therefore stops at evidence-based baseline analysis. Lab 30 should use H7USER as the controlled actor. Lab 31 can later classify UID(0) identities as KEEP / REVIEW / CANDIDATE FOR REDUCTION with rollback requirements.
