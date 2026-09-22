# Lab 36 — TN3270 Cleartext Transport Exposure Validation

## Objective

Validate, with packet-level evidence in the controlled z/OS ADCD / Hercules laboratory, whether the tested TN3270 access path exposes Telnet/TN3270E negotiation and 3270 application data without a TLS layer.

This lab follows Lab 35. Lab 35 validated authentication-response differences at the TSO logon surface. Lab 36 moves one layer down and validates the transport exposure of that same interactive access path.

The lab does **not** capture passwords, perform credential interception, or claim a production vulnerability. It validates the protocol and transport behavior observed in this specific laboratory.

## Context

Previous repository work established the security relevance of TN3270/TSO authentication behavior. The Communications Server portfolio separately established the TN3270 runtime/configuration baseline and the TCP/23 listener.

During Lab 36, direct access from the external workstation to the z/OS LCS address was not available, while the Hercules host could reach the z/OS TN3270 listener. A temporary TCP relay was therefore used only as a controlled reachability aid.

The capture was made on the client-to-relay leg. The relay did not introduce TLS or transform the application protocol.

## Architecture

```text
Kali / c3270
     |
     | TCP/<RELAY_PORT>
     v
Hercules Windows host
temporary TCP relay
     |
     | TCP/23
     v
z/OS Communications Server
TN3270
     |
     v
VTAM / TSO
```

Important boundary:

```text
captured:
client <-> temporary relay

not directly captured:
relay <-> z/OS TCP/23
```

The lab therefore documents the tested topology explicitly rather than presenting the capture as a transparent packet capture of the z/OS ETH1 link.

## Evidence-driven flow

### 1. TN3270 runtime baseline

The z/OS runtime evidence showed the TN3270 address space active and the service listening on classic TCP port 23.

```text
TN3270 ... 0.0.0.0..23 ... LISTEN
```

### 2. z/OS TCP/IP and routing baseline

`NETSTAT,HOME` and `NETSTAT,ROUTE` established the active ETH1 path and routing state.

Private addressing is pixelated in the public evidence set.

### 3. Reachability boundary

The Hercules host could reach the z/OS TN3270 listener, while the external workstation could not reach the z/OS LCS address directly.

This separated the transport-exposure question from the previously known LCS/external-reachability limitation.

### 4. Temporary controlled relay

A temporary Windows `portproxy` rule mapped a non-standard lab-only listening port to z/OS TCP/23. A matching temporary inbound firewall rule was used only for the duration of the test.

The relay was removed at the end of the lab and cleanup was verified.

### 5. Remote TN3270 session

`c3270` successfully reached the ADCD VTAM entry screen through the controlled relay.

No userid or password was entered for the packet-capture validation.

### 6. Packet capture

`tcpdump` captured the client-side stream. The packet payload exposed Telnet negotiation data and the negotiated terminal string:

```text
IBM-3278-3-E
```

The captured exchange also contained the TN3270E Telnet option negotiation and a 3270 data stream.

### 7. TLS observation

No TLS handshake preceded the Telnet/TN3270E negotiation on the tested path.

The observed sequence was:

```text
TCP handshake
     |
     v
Telnet option negotiation
     |
     v
TN3270E / IBM-3278-3-E
     |
     v
3270 application data
```

not:

```text
TCP handshake
     |
     v
TLS handshake
     |
     v
encrypted application stream
```

## Result

**PASS — cleartext TN3270 transport exposure was validated on the tested path.**

The evidence demonstrates:

- an active TN3270 service;
- classic TCP/23 runtime exposure on z/OS;
- a successful remote TN3270 session;
- observable Telnet/TN3270E negotiation;
- observable `IBM-3278-3-E` terminal information;
- no TLS handshake on the tested stream;
- controlled relay cleanup after validation.

## Security interpretation

The result should be described as a transport-exposure finding in the controlled laboratory.

The packet stream exposed protocol metadata and TN3270E application negotiation without TLS. This demonstrates why classic TN3270 should be reviewed as a cleartext transport unless protected by TLS/AT-TLS or an equivalent secure transport design.

The lab does **not** claim:

- recovered credentials;
- account compromise;
- a RACF bypass;
- a z/OS product vulnerability;
- direct packet interception on the z/OS ETH1 segment;
- behavior identical to every z/OS installation.

## Relationship to Lab 35

```text
Lab 35
TN3270 / TSO authentication-response exposure
        |
        v
Lab 36
TN3270 cleartext transport exposure
        |
        v
next video block
OMVS / Netcat
```

Lab 35 studied what the authentication surface reveals through response behavior.

Lab 36 studies how the underlying TN3270 transport is presented on the network.

## Repository boundary

- `mainframe-racf-security-evidence` owns the security interpretation and evidence chain.
- `zos-communications-server-network-lab` remains the owner of TN3270 configuration, TCP/IP engineering, LCS connectivity, Policy Agent and AT-TLS implementation work.

## Contents

- `commands/lab36-commands.txt` — sanitized command sequence and cleanup.
- `docs/findings.md` — evidence-derived findings.
- `docs/security-analysis.md` — risk interpretation and limitations.
- `docs/troubleshooting.md` — reachability, relay and capture troubleshooting.
- `docs/video-mapping.md` — mapping to the source-video interception section.
- `docs/rollback.md` — temporary relay cleanup and verification.
- `docs/next-step.md` — controlled continuation into the OMVS/Netcat block.
- `evidence/README.md` — public evidence manifest.
- `evidence/screenshots/` — pixelated publication-safe screenshots.

## Closure

Lab 36 closes the packet-level validation of the source video's TN3270 interception concept without collecting credentials or reproducing uncontrolled interception.

The temporary relay and firewall rule were removed after testing. Raw packet-capture data remains local and is not included in the public evidence set.
