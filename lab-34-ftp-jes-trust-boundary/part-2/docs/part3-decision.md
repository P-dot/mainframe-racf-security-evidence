# Decision on a Possible Part 3

## Decision

**Do not make Part 3 the immediate next lab.**

Part 2 has already answered the question that originated in the FTP segment of the source video: a valid harmless job can be submitted through the FTP JES interface and execute as the authenticated RACF user even though the same user is denied the TSO/E `SUBMIT` command.

The remaining `JESINPUT` / `JESJOBS` question is valuable, but it is now a deeper platform-hardening and effective-SAF-path investigation rather than a prerequisite for following the video's sequence.

## Why defer it

The video moves on after the FTP/JES demonstration to different attack-surface themes, including service fingerprinting/enumeration, TSO/TN3270 user enumeration/brute-force concepts, and later OMVS/Netcat-style tooling.

Continuing to drill into internal-reader SAF semantics now would improve completeness but would also move away from the video's progression.

## Recommended backlog item

A future optional extension can be created as:

```text
Lab 34 — Part 3 (optional)
Effective JES Security Path Analysis
```

Possible scope:

- establish the exact SAF checks performed for the FTP JES level-1 submission path;
- correlate RACF class state with JES2 initialization and FTP server behavior;
- collect audit evidence for the authorization decision;
- test a minimal controlled hardening change only if justified;
- validate rollback.

No broad privilege should be granted merely to manufacture a positive or negative result.

## Recommended immediate continuation

Return to the video sequence and open a new security lab around **TN3270/TSO authentication exposure and user-enumeration resistance**, using controlled identities and defensive validation rather than reproducing uncontrolled brute-force activity.
