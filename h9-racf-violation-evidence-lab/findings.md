# Findings — H9 RACF Violation Evidence Lab

## Finding 1 — Controlled RACF violation captured

A controlled access denial was generated using `H7USER` against the sandbox dataset area.

The violation appeared in SYSLOG as `ICH408I`, identifying insufficient access authority.

## Finding 2 — PRIVATE profile denies correctly

`IBMUSER.SECLAB.PRIVATE.*` is configured with:

```text
UACC(NONE)
```

No explicit H7USER access list entry was present, so denial was expected.

## Finding 3 — AUDIT profile is prepared for read failure evidence

`IBMUSER.SECLAB.AUDIT.*` is configured with:

```text
UACC(NONE)
AUDIT(FAILURES(READ))
```

This supports controlled failure evidence, while full forensic extraction still depends on SMF readiness.

## Finding 4 — GRANTED profile permits by explicit access list

`IBMUSER.SECLAB.GRANTED.*` includes:

```text
H7USER READ
```

This proves that H7USER is not simply blocked everywhere. RACF permits access when a valid access list entry exists.

## Finding 5 — SMF status remains a forensic-readiness consideration

The `/D SMF` capture shows MAN dataset status, including datasets marked `DUMP REQUIRED` and alternate datasets. This is relevant for future work on SMF hygiene and RACF event retention.
