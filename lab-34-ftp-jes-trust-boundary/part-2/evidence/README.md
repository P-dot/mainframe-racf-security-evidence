# Guided Evidence — Lab 34 — Part 2: Channel-Dependent FTP-to-JES Authorization Validation

[← Lab lesson](../README.md) · [Academy](https://github.com/P-dot/P-dot/blob/main/docs/ACADEMY.md) · [Evidence standard](https://github.com/P-dot/P-dot/blob/main/docs/LAB-STANDARD.md)

## How to read this evidence

This page is the evidence companion to the lab, not a screenshot gallery. Read the artifacts in execution order and correlate each image or file with the command, job, subsystem state or result described by the lab.

Use four questions while reviewing the evidence:

1. **Intent** — what state or behavior was the lab trying to create or inspect?
2. **Mechanism** — which z/OS component, command, utility or program performed the work?
3. **Observation** — what concrete message, return code, object or state was captured?
4. **Boundary** — what does that artifact support, and what would require additional evidence?

The manifest below is retained as the factual index from the executed lab. Its descriptions are the source of truth for what each artifact was captured to demonstrate.

## Evidence manifest

# Part 2 Evidence Manifest

The screenshots in this directory are extracted from the user's captured z/OS lab session and are retained as direct evidence for the Part 2 conclusions.

| File | Evidence |
|---|---|
| `01-ftpjes2-jcl.png` | Minimal valid `FTPJES2` / `IEFBR14` member |
| `02-tso-submit-denied.png` | `H7USER` denied TSO/E `SUBMIT` |
| `03-ftp-jes-job07465-submitted.png` | FTP/JES internal-reader handoff and `JOB07465` assignment |
| `04-tsoauth-jcl-profile.png` | `TSOAUTH JCL` profile baseline |
| `05-h7user-no-privileged-attributes.png` | `LISTUSER H7USER`, `ATTRIBUTES=NONE` |
| `06-jesjobs-no-profiles.png` | `SEARCH CLASS(JESJOBS)` returns no entries |
| `07-job07465-identity-and-completion.png` | JES log assigns H7USER and shows job start/end |
| `08-job07465-jesjcl.png` | JESJCL confirms submitted two-line job |
| `09-job07465-cc0000.png` | JESYSMSG confirms `COND CODE 0000` |
| `10-jesinput-no-profiles.png` | `SEARCH CLASS(JESINPUT)` returns no entries |
| `11-surrogat-profile-search.png` | SURROGAT inventory includes `IBMUSER.SUBMIT` |
| `12-surrogat-ibmuser-submit-profile.png` | `IBMUSER.SUBMIT` profile baseline |
| `13-surrogat-no-h7user-access.png` | Access list shows no H7USER grant |
| `14-rdi-no-selectable-entries.png` | Point-in-time RDI display after job completion |
| `15-intrdr-batch-enabled.png` | `$D INTRDR` shows `BATCH=YES`, class A and current characteristics |

## Publication review

No password value is shown in the selected Part 2 evidence. The retained network reference is loopback (`127.0.0.1`) in the test flow. Before publication, continue to review screenshots for any unrelated terminal/session identifiers or environmental detail that is not necessary to the technical conclusion.

## Interpretation discipline

A successful command, return code or panel is interpreted only within the scope described by the lab. It must not be promoted into proof of unrelated production properties such as availability, performance, security hardening or recovery unless those properties have their own evidence.

When troubleshooting, walk the artifacts in order and locate the first point where **expected state** and **observed state** diverge. That point is normally more useful than the final symptom.

## Evidence boundary

**Evidence-backed:** the individual observations explicitly identified in the manifest and the parent lab.

**Not automatically implied:** production readiness, enterprise scale, security completeness, performance characteristics or cross-subsystem behavior that was not exercised by this lab.

## Review questions

- Which artifact establishes the initial or prerequisite state?
- Which artifact is the strongest execution/result proof?
- Is there a separate final-state validation, or only a successful command?
- Which z/OS subsystem owns the observed messages or objects?
- What additional artifact would be required to make a stronger claim?

---
### Continue learning

**Lab:** [Return to the lesson](../README.md)  
**Academy:** [z/OS Engineering Academy](https://github.com/P-dot/P-dot/blob/main/docs/ACADEMY.md) · [Curriculum](https://github.com/P-dot/P-dot/blob/main/docs/CURRICULUM.md) · [Relationships](https://github.com/P-dot/P-dot/blob/main/docs/RELATIONSHIPS.md)
