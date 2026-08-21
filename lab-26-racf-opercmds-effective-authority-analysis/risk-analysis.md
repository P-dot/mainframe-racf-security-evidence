# Lab 26 — Risk Analysis

## Risk statement

A generic `MVS.**` `OPERCMDS` profile in `WARNING` mode represents incomplete enforcement coverage until its impact is understood and warning mode is deliberately accepted or removed under change control.

## Existing controls observed

- `OPERCMDS` active.
- `MVS.SETPROG` specific profile with UACC(NONE), WARNING(NO).
- `MVS.SET.PROG` specific profile with UACC(NONE), WARNING(NO).
- Explicit IBMUSER READ entries.
- Audit setting `FAILURES(READ)`.
- Effective denial of `SETPROG APF,DISPLAY`.

## What is *not* claimed

This lab does not claim:

- unrestricted command authority;
- that every command covered by `MVS.**` can be executed;
- that `MVS.** WARNING(YES)` is by itself a confirmed vulnerability;
- that READ on an `OPERCMDS` profile maps directly to every command function;
- that remediation can safely be applied without impact analysis.

## Recommended next step

Lab 27 should perform controlled `OPERCMDS` hardening design:

1. establish exact intended command ownership and access requirements;
2. document before-state and rollback;
3. evaluate generic `MVS.**` warning dependencies;
4. introduce only justified granular controls;
5. refresh only where required;
6. repeat positive and negative command tests;
7. confirm no unintended operational regression.
