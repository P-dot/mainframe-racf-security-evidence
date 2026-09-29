# Lab 37 — Part 1: OMVS / NC110 Readiness and LCS/ETH1 Connectivity Blocker

## Status

**CLOSED — PART 1 / READINESS VALIDATED / NETWORK BLOCKER DOCUMENTED**

Part 1 intentionally stops before NC110 execution.

The objective was to begin the source video's **Netcat on Mainframe / OMVS** block, establish whether this z/OS V1R11 ADCD environment could support the experiment, and identify any prerequisite that prevented the controlled Netcat validation from continuing.

The result is useful even though Netcat was not yet executed: OMVS, the native TCP/IP view and the C/C++ toolchain were validated, while the external LCS/ETH1 path was isolated as the blocker.

---

## Video position

The security-series progression is now:

```text
Lab 34
FTP -> JES trust boundary
        |
        v
Lab 35
TN3270 / TSO authentication-response exposure
        |
        v
Lab 36
TN3270 cleartext transport exposure
        |
        v
Lab 37 — Part 1
OMVS / NC110 readiness
+ LCS/ETH1 connectivity blocker
        |
        v
Lab 37 — Part 2
NC110 compile + controlled TCP exchange
+ EBCDIC/ASCII observation
```

Part 1 does not jump ahead to shell execution, credential handling, CATSO, TShocker or later source-video material.

---

## Objective

Validate the prerequisites for a controlled OMVS/Netcat experiment:

```text
OMVS
  |
  +--> TCP/IP visibility
  |
  +--> native compiler toolchain
  |
  +--> NC110 source staging
  |
  +--> external network path
           |
           +--> required for transfer/test
```

The lab was expected to proceed toward:

```text
NC110-OMVS source
      |
      v
z/OS XL C/C++
      |
      v
./nc
      |
      v
controlled OMVS <-> lab-client TCP exchange
```

The final stage was not reached because the host-to-z/OS LCS/ETH1 path became unavailable.

---

# Evidence-driven sequence

## 1. OMVS baseline

The z/OS UNIX shell was available under the controlled ADCD identity.

Initial discovery checked for:

```text
nc
netcat
ncat
telnet
python
python3
```

No usable Netcat/Python path was identified in the checked standard locations.

This is important because the source-video exercise cannot simply assume a modern `nc` or Python installation on an older z/OS image.

Evidence:

- `01-omvs-command-discovery.png`

---

## 2. Native TCP/IP visibility from OMVS

Both commands resolved successfully:

```text
/bin/netstat
/bin/onetstat
```

`netstat -a` exposed the active z/OS TCP/IP socket table from the UNIX shell.

Observed listeners included, among others:

```text
FTPD1   TCP/21
SSHD4   TCP/22
TN3270  TCP/23
HTTPD1  TCP/80
PORTMAP TCP/111
```

The wildcard address `0.0.0.0` is retained in the screenshots because it is protocol/listener state, not a private host address.

Evidence:

- `02-omvs-netstat-tcp-baseline.png`
- `03-omvs-netstat-service-listeners.png`

---

## 3. Native compiler readiness

The following commands were present:

```text
/bin/make
/bin/cc
/bin/c89
/bin/xlc
```

The version query confirmed:

```text
z/OS V1.11 XL C/C++
```

`make -v` also revealed an environment-specific issue:

```text
FSUM9383 Configuration file '/etc/startup.mk' not found
```

This does not prove that the compiler is unavailable. `xlc -qversion` succeeded, while the `cc -V` / `c89 -V` invocations were interpreted as compiler invocations without source operands.

The important readiness result is that a native XL C/C++ toolchain exists.

Evidence:

- `04-native-compiler-toolchain.png`
- `05-make-startupmk-and-xlc-version.png`

---

## 4. Controlled USS workspace

A dedicated workspace was created:

```text
/u/ibmuser/lab37-nc110
```

No system directory was modified.

Evidence:

- `06-lab37-uss-work-directory.png`

---

## 5. NC110 source staging

The external Windows lab host successfully obtained the two source files needed for the next stage:

```text
netcat.c
generic.h
```

The source was taken from the public `mainframed/NC110-OMVS` project.

This establishes that the next intended action was source transfer and compilation, not use of an unidentified prebuilt binary.

Evidence:

- `07-nc110-source-download-windows.png`

Reference:

```text
https://github.com/mainframed/NC110-OMVS
```

---

# The blocker

## 6. FTP transfer could not begin

The planned source-transfer path was:

```text
Windows lab host
       |
       | FTP / TCP 21
       v
z/OS FTPD1
       |
       v
/u/ibmuser/lab37-nc110
```

Although `FTPD1` was visible internally as a listener, the host could not establish the external TCP/21 connection.

Evidence:

- `08-ftp-transfer-timeout-sanitized.png`

---

## 7. The failure was below FTP

The same host also failed against TN3270/TCP 23, and ICMP reachability failed.

Therefore the failure was not classified as an FTP application problem.

```text
FTP/21       FAIL
TN3270/23    FAIL
ICMP         FAIL
```

The failure boundary moved downward:

```text
application service
        |
        v
z/OS TCP/IP
        |
        v
ETH1 / LCS
        |
        X
host/emulator integration
```

Evidence:

- `09-host-to-zos-reachability-failure-sanitized.png`

---

# Host-side recovery investigation

## 8. KM-TEST / loopback driver state

Windows retained the Microsoft loopback driver definition in:

```text
C:\Windows\INF\netloop.inf
```

The INF identified:

```text
*MSLOOP
Microsoft KM-TEST Loopback Adapter
```

The previously registered loopback devnode was initially observed in an error / not-present state.

A controlled PnP repair then:

- re-applied `netloop.inf`;
- enabled the existing device;
- restarted the device;
- rescanned hardware.

After that operation the devnode reported `OK` and the lab adapter reported `Up`.

This fixed the Windows PnP state, but **did not restore z/OS external reachability**, proving that the remaining problem was deeper than merely the adapter's presence in Device Manager.

Evidence:

- `10-netloop-kmtest-driver-definition.png`

---

# Hercules / z/OS boundary

## 9. Hercules LCS device state

Hercules exposed the expected LCS pair:

```text
0E20
0E21
```

The base device was initially reported as busy, preventing an immediate reinitialization.

Evidence:

- `11-hercules-lcs-device-busy-sanitized.png`

---

## 10. Controlled z/OS LCS stop

The z/OS TCP/IP command:

```text
V TCPIP,,STOP,LCS1
```

completed successfully.

After the stop, `NETSTAT,DEVLINKS` showed the loopback path while the LCS path was no longer active.

This was a controlled operational step used to release the LCS device before retrying Hercules-side initialization.

Evidence:

- `12-zos-controlled-lcs-stop.png`

---

## 11. Hercules backend initialization failure

Once the device was no longer busy, Hercules accepted the reinitialization attempt far enough to expose the actual backend error:

```text
HHCTU005E Invalid net device name specified
HHCLC008E ioctl error on device : Bad file descriptor
```

This is the strongest blocker evidence in Part 1.

It shows that the current failure is not simply "TCP port 21 closed" or "Netcat missing". The emulated LCS device cannot currently establish its host networking backend correctly.

Private host/guest addresses in the command are pixelated in the public screenshot.

Evidence:

- `13-hercules-devinit-backend-error-sanitized.png`

---

## 12. LCS1 could be started again

The controlled command:

```text
V TCPIP,,START,LCS1
```

completed successfully.

The z/OS HOME state continued to retain the ETH1 identity, but external reachability was still unavailable.

This distinction is important:

```text
z/OS configuration identity present
               !=
working host-to-guest data path
```

Evidence:

- `14-zos-lcs-start-home-state-sanitized.png`

---

# Findings

Part 1 establishes the following:

```text
OMVS shell                     VALIDATED
netstat / onetstat             VALIDATED
TCP/IP listener visibility     VALIDATED
XL C/C++ toolchain             VALIDATED
NC110 source acquisition       VALIDATED
dedicated USS workspace        VALIDATED

NC110 source transfer          BLOCKED
NC110 compilation              NOT EXECUTED
./nc execution                 NOT EXECUTED
OMVS <-> external TCP test     NOT EXECUTED
EBCDIC/ASCII experiment        NOT EXECUTED

external z/OS reachability     UNAVAILABLE
LCS backend initialization     FAILED
```

---

# Security interpretation

The security-relevant finding of Part 1 is not a Netcat vulnerability.

The lab demonstrates a prerequisite boundary:

```text
security experiment
      |
      v
OMVS process/socket capability
      |
      v
z/OS TCP/IP
      |
      v
LCS/ETH1 transport
      |
      v
emulator / host networking
```

A security test that depends on network reachability cannot be interpreted correctly if the underlying emulated network attachment is broken.

This prevents false conclusions such as:

```text
"FTP is blocked"
"TN3270 is down"
"Netcat cannot work on z/OS"
```

when the evidence actually points to the LCS host-backend boundary.

---

# Cross-repository boundary

This remains **Lab 37 Part 1 in the security series** because it records the preparation and blocker encountered while following the source video's Netcat-on-mainframe security path.

However, detailed remediation of LCS/ETH1 belongs primarily to:

```text
P-dot/zos-communications-server-network-lab
```

That repository already owns:

- TCP/IP profile engineering;
- `LCS1` / `ETH1`;
- Hercules host-network integration;
- external-connectivity investigation.

The security repository records only enough infrastructure troubleshooting to explain why the video-driven test stopped.

---

# What Part 1 does NOT claim

Part 1 does not claim that:

- NC110 was compiled;
- Netcat executed on z/OS;
- a listener was created;
- an outbound Netcat session succeeded;
- a shell was spawned;
- EBCDIC/ASCII translation was demonstrated;
- a RACF bypass occurred;
- the network failure is a z/OS product vulnerability.

---

# Part 2 handoff

Part 2 should begin only after the LCS/ETH1 path is restored.

The continuation is deliberately narrow:

```text
restore external reachability
        |
        v
transfer netcat.c + generic.h
        |
        v
compile NC110-OMVS
        |
        v
./nc -h
        |
        v
controlled text-only TCP exchange
        |
        v
observe ASCII / EBCDIC behavior
        |
        v
evaluate NetEBCDICat.py
```

No later video techniques should be pulled into Part 2 before this controlled foundation works.

---

# Contents

- `commands/lab37-part1-commands.txt`
- `docs/findings.md`
- `docs/security-analysis.md`
- `docs/troubleshooting.md`
- `docs/video-mapping.md`
- `docs/part2-handoff.md`
- `evidence/README.md`
- `evidence/screenshots/`

---

# Closure

**Lab 37 Part 1 is closed as a readiness-and-blocker investigation.**

The work stayed aligned with the source video's OMVS/Netcat block until the environment prevented source transfer and external socket testing.

Rather than bypassing or hiding that limitation, Part 1 preserves the evidence and establishes an exact restart point for Part 2.
