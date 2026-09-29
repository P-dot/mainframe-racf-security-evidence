# Source-Video Mapping

## Source-video concept

The next block after the TN3270 interception material moves into Netcat behavior from z/OS UNIX / OMVS.

Part 1 maps only the prerequisites and the environment blocker encountered before NC110 could run.

```text
video:
Netcat on Mainframe / OMVS
        |
        v
our Part 1:
OMVS baseline
        |
        v
TCP/IP visibility
        |
        v
compiler readiness
        |
        v
NC110 source staging
        |
        X
LCS/ETH1 external-connectivity blocker
```

## Deliberate stop point

Part 1 does not absorb later source-video topics such as:

```text
NetEBCDICat.py
MainTP.py
CATSO
TShocker
```

Those remain downstream.

## Resume point

Once external connectivity is restored:

```text
transfer NC110 source
 -> compile
 -> verify ./nc
 -> controlled text-only TCP exchange
 -> observe EBCDIC/ASCII
```

That will be Lab 37 Part 2.
