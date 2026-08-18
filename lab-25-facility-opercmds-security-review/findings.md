# Lab 25 — Security Findings

## F25-01 — STGADMIN baseline

Observed:

- UACC(NONE)
- WARNING(NO)
- FAILURES(READ) auditing
- restricted explicit access

Classification:

KEEP / BASELINE

No evidence collected in this lab justifies changing the reviewed
STGADMIN profiles.


## F25-02 — BPX.SUPERUSER technical authorization

Observed:

- UACC(NONE)
- IBMUSER ALTER
- CIMSG READ

Classification:

REVIEW

CIMSG authorization should be mapped to its technical function before
any remediation decision.


## F25-03 — BPX.SERVER technical identities

Observed:

- UACC(NONE)
- multiple explicit technical-account permissions

Classification:

REVIEW

The accounts must be correlated with the services they support before
least-privilege changes are considered.


## F25-04 — BPX.FILEATTR.APF

Observed:

- UACC(NONE)
- explicit privileged access including AOXDB ALTER

Classification:

REVIEW

FILEATTR.APF is security sensitive. The authorization requires functional
justification before retention or removal can be recommended.


## F25-05 — BPX.FILEATTR.PROGCTL

Observed:

- UACC(NONE)
- explicit permissions for multiple identities
- AOXDB ALTER observed

Classification:

REVIEW

The explicit permissions require service-owner and functional validation.


## F25-06 — OPERCMDS granular-control gap candidate

Previous Lab 12 evidence found no matching profiles for the tested
OPERCMDS search patterns while tested SDSF display commands returned
operational output.

Classification:

HARDEN CANDIDATE

This finding does NOT demonstrate that arbitrary users can execute
arbitrary operator commands.

Effective authority and existing control paths must be established before
new OPERCMDS profiles are defined.
