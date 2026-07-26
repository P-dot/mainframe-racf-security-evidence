# Lab 14 — Final RACF / OMVS Security Audit Report & Hardening Roadmap

## Purpose

This lab closes the RACF / OMVS security audit performed on the local ADCD z/OS 1.11 environment running under Hercules.

Unlike the previous labs, this final lab does not introduce new RACF, SDSF, OMVS, JES2, or APF commands. Its purpose is to consolidate the evidence already collected into a professional audit-style report.

## Audit position

This repository documents an educational security assessment of a lab mainframe environment. It should not be presented as a production penetration test or as a production compliance certification.

The value of the work is that it demonstrates the ability to:

- identify privileged RACF users and technical IDs;
- connect started tasks with RACF identities;
- review OMVS / UID(0) exposure;
- inspect sensitive DATASET profile coverage;
- review zFS backing datasets;
- examine SAF general resource classes such as FACILITY, UNIXPRIV, SERVAUTH, OPERCMDS, JESSPOOL, JESJOBS and SDSF;
- assess audit/logging/accountability posture;
- review APF, PROGRAM and LINKLIST exposure;
- convert raw mainframe evidence into a risk-based hardening roadmap.

## Main conclusion

The ADCD environment is functional and useful for learning, but the collected evidence does not show a hardened production-style RACF / OMVS security baseline.

The dominant pattern is:

```text
privileged technical IDs
+ shared or repeated UID(0) exposure
+ sensitive datasets without visible RACF DATASET profiles
+ limited visible granular SAF controls
+ incomplete/limited audit evidence
+ active APF and LINKLIST libraries requiring strict protection
```

## Final deliverables

- `executive-summary.md`
- `audit-scope.md`
- `consolidated-findings.md`
- `risk-register.md`
- `evidence-mapping.md`
- `hardening-roadmap.md`
- `production-recommendations.md`
- `adcd-limitations.md`
- `recruiter-summary.md`
- `github-upload-commands.md`
- `evidence/references-to-prior-labs.md`
