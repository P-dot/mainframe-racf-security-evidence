# Lab 37 Part 1 — Findings

## F1 — OMVS is usable

The controlled ADCD identity reached a functioning z/OS UNIX shell and a dedicated lab directory could be created under `/u/ibmuser`.

## F2 — Modern Netcat/Python assumptions do not hold

The checked paths did not expose `nc`, `netcat`, `ncat`, `telnet`, `python`, or `python3`.

The lab therefore moved toward source-based NC110 rather than assuming a preinstalled modern userland.

## F3 — z/OS TCP/IP is visible from OMVS

`/bin/netstat` and `/bin/onetstat` exist and expose the TCP/IP socket table.

Observed listeners included FTP, SSH, TN3270, HTTP and PORTMAP.

## F4 — The native compiler prerequisite exists

`make`, `cc`, `c89`, and `xlc` were present.

`xlc -qversion` identified `z/OS V1.11 XL C/C++`.

`make -v` exposed a missing `/etc/startup.mk` configuration file, which is an environment-specific build consideration rather than evidence that XL C/C++ is absent.

## F5 — NC110 source staging succeeded off-mainframe

`netcat.c` and `generic.h` were obtained on the controlled Windows lab host.

They were not transferred to USS because network reachability failed before the FTP session could be established.

## F6 — The transfer failure was not FTP-specific

TCP/21, TCP/23 and ICMP all failed from the host toward the z/OS guest address.

This moved the fault domain below FTP/TN3270 and toward the host/emulator attachment.

## F7 — Repairing the Windows loopback devnode was insufficient

The Microsoft KM-TEST loopback devnode moved from an error/not-present state to `OK`, and the associated lab adapter moved to `Up`.

External z/OS reachability still failed.

## F8 — z/OS could stop and restart LCS1 cleanly

`V TCPIP,,STOP,LCS1` and `V TCPIP,,START,LCS1` completed successfully.

The problem was therefore not documented as a failure to process those TCP/IP operational commands.

## F9 — Hercules exposed the concrete backend failure

After LCS1 was stopped, the Hercules device could be retried and returned:

```text
HHCTU005E Invalid net device name specified
HHCLC008E ioctl error on device : Bad file descriptor
```

This is the strongest Part 1 evidence for the current blocker.

## Final Part 1 finding

The z/OS/OMVS and native compiler prerequisites for the Netcat-on-mainframe exercise are present.

The experiment cannot yet proceed because the LCS/ETH1 host-backend path is not operational.

No conclusion about NC110 runtime behavior is made in Part 1.
