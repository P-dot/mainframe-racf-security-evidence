# Next Steps

Recommended follow-up lab:

```text
Lab 13 — APF / PROGRAM / Authorized Libraries Review
```

Why this comes next:

- Labs 07–11 identified identity, OMVS, dataset, zFS, SAF and audit exposure.
- Lab 12 reviewed operational control around JES2, SDSF and commands.
- The next major escalation path is authorized code: APF libraries, program control and authorized load libraries.

Proposed read-only checks:

```text
/D PROG,APF
D PROG,LNKLST
LISTDSD DA('<APF_LIBRARY>') ALL
SEARCH CLASS(PROGRAM) MASK(*)
RLIST PROGRAM <profile> ALL
```

No APF or PROGRAM changes should be made in the next lab. It should remain evidence-only.
