# Findings — Lab 31

## F31-01 — Explicit protection exists for RACDCERT listing functions

Observed profiles:

```text
IRR.DIGTCERT.LIST
IRR.DIGTCERT.LISTRING
```

Observed protection:

```text
UACC(NONE)
IBMUSER ALTER
WSCFG1 READ
XSCFG READ
AUDIT FAILURES(READ)
```

Interpretation: the environment does not expose these observed RACDCERT listing controls through broad default access.

## F31-02 — Several queried administrative IRR.DIGTCERT resources are not defined

The following queried profiles returned not-found results:

```text
IRR.DIGTCERT.GENCERT
IRR.DIGTCERT.ADD
IRR.DIGTCERT.ADDRING
IRR.DIGTCERT.CONNECT
```

Interpretation: absence of these profiles is a configuration fact, not proof of effective authorization.

## F31-03 — H7USER cannot issue the tested RACDCERT functions

Observed result:

```text
IRRD101I You are not authorized to issue the RACDCERT command.
```

The same controlled identity was unable to list its own rings, list IBMUSER rings, or create the laboratory test ring through the tested RACDCERT path.

## F31-04 — No uncontrolled key-ring creation occurred

`LAB31RING` was not created.

No cleanup was required.

## F31-05 — SITE inventory gap from prior network work is closed

`RACDCERT SITE LIST` returned no SITE certificate information in this observed environment.

This complements, rather than duplicates, the Communications Server certificate/key-ring inventory.
