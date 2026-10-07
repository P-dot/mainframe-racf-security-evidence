# Guided Evidence — Lab 37 — Part 1: OMVS / NC110 Readiness and LCS/ETH1 Connectivity Blocker

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

# Evidence Manifest — Lab 37 Part 1

The screenshots preserve the original command/output layout. Only private network or host-identifying fields are pixelated where needed.

| # | File | What it proves |
|---|---|---|
| 01 | `01-omvs-command-discovery.png` | OMVS baseline and absence of assumed modern tools in checked paths |
| 02 | `02-omvs-netstat-tcp-baseline.png` | `netstat` / `onetstat` availability and z/OS TCP/IP socket visibility |
| 03 | `03-omvs-netstat-service-listeners.png` | FTP, SSH, TN3270 and other active listeners |
| 04 | `04-native-compiler-toolchain.png` | `make`, `cc`, `c89`, `xlc` binaries available |
| 05 | `05-make-startupmk-and-xlc-version.png` | `/etc/startup.mk` issue plus successful XL C/C++ version identification |
| 06 | `06-lab37-uss-work-directory.png` | Dedicated `/u/ibmuser/lab37-nc110` workspace |
| 07 | `07-nc110-source-download-windows.png` | `netcat.c` and `generic.h` staged externally |
| 08 | `08-ftp-transfer-timeout-sanitized.png` | FTP transfer path timed out |
| 09 | `09-host-to-zos-reachability-failure-sanitized.png` | TCP/21, TCP/23 and ICMP all failed, moving diagnosis below FTP |
| 10 | `10-netloop-kmtest-driver-definition.png` | Inbox KM-TEST / `*MSLOOP` driver definition present |
| 11 | `11-hercules-lcs-device-busy-sanitized.png` | Hercules LCS pair present and base device initially busy |
| 12 | `12-zos-controlled-lcs-stop.png` | Controlled `V TCPIP,,STOP,LCS1` completed |
| 13 | `13-hercules-devinit-backend-error-sanitized.png` | Backend error after device release: `HHCTU005E` / `HHCLC008E` |
| 14 | `14-zos-lcs-start-home-state-sanitized.png` | Controlled LCS1 restart and retained HOME identity |

## Sanitization policy

Pixelated where present:

- private IPv4 addresses;
- hostnames;
- MAC addresses or host adapter identifiers if selected evidence exposed them.

Retained because they are part of the technical finding:

- `0.0.0.0` wildcard listener addresses;
- TCP service ports;
- z/OS job/service names;
- LCS device numbers;
- commands and error identifiers.

No raw network capture or credential material is included.

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
