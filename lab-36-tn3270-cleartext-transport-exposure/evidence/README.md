# Guided Evidence — TN3270 Cleartext Transport Exposure

[← Lab](../README.md) · [Previous boundary: authentication](../../lab-35-tn3270-tso-authentication-exposure/evidence/README.md) · [Communications Server](https://github.com/P-dot/zos-communications-server-network-lab)

This lab separates **authentication security** from **transport security**. Lab 35 showed that a user can reach TSO/RACF authentication. This lab asks a different question: **what protects the bytes while that 3270 session crosses the network?**

## Layer model

    user authentication
         RACF
           ^
           |
    TSO / VTAM application
           ^
           |
        TN3270E
           ^
           |
          TCP
           ^
           |
      network path

A valid RACF password check does not encrypt TCP. Identity controls and transport controls solve different problems.

## Evidence chain

| Evidence | What to observe | What it establishes |
|---|---|---|
| 01 address-space image | TN3270 address space active | Server runtime exists |
| 02 listener image | Classic TCP/23 listener | Service is exposed on expected port |
| 03 HOME image | z/OS HOME configuration | IP endpoint context, private address removed |
| 04 route image | z/OS routing state | Network path exists at IP layer |
| 05 connectivity image | host reachability/TCP test | Client side can reach listener |
| 06 portproxy image | controlled temporary relay | Lab path crosses intended host boundary |
| 07 c3270 image | remote VTAM entry screen | Application session is usable remotely |
| 08 tcpdump image | packets during session | Traffic is observable on path |
| 09 negotiation image | IBM-3278-3-E negotiation | Captured protocol identified as TN3270E |
| 10 diagnostics image | host network diagnostics | Supporting path/interface evidence |

## How to interpret the packet evidence

The critical proof is not merely that packets exist. Evidence 09 identifies TN3270E negotiation semantics in captured traffic. Together with the classic TCP/23 listener and successful remote terminal session, the lab demonstrates an observed TN3270/TN3270E path without a demonstrated TLS protection layer.

**Scoped conclusion:** the tested TN3270 path exposes application-protocol traffic in a form observable on the network path; no TLS protection is demonstrated by this evidence.

That is more precise than saying only that port 23 is insecure, and safer than making claims about traffic that was not captured.

## Security handoff

    Communications Server
      listener / routing
             |
             v
       TN3270 transport
             |
      +------+------+
      |             |
    RACF         TLS policy
   identity       transport
      |             |
      +------v------+
        session risk

This is the Academy bridge between the networking and security repositories. RACF can authenticate identity while transport requires separate protection.

## Production reasoning

A production remediation path would investigate a protected TN3270 design and the appropriate Communications Server security policy. This evidence documents the **exposure baseline**; it does not claim remediation has already been implemented.

## Publication safety

All publication images are sanitized. Private network data, MAC addresses, hostnames, terminal/session identifiers and local paths are pixelated where they could disclose the laboratory network.

The raw PCAP remains local because it contains real addressing and packet metadata unnecessary for public proof. Sanitized screenshots retain the protocol evidence needed to teach the finding.

## Evidence boundary

**VALIDATED:** TN3270 runtime, TCP/23 listener, remote reachability, usable 3270 session, packet observation and TN3270E protocol negotiation evidence.

**NOT CLAIMED:** AT-TLS remediation, encrypted TN3270, certificate validation, production firewall policy or enterprise network segmentation.

## Knowledge check

1. Why does successful RACF authentication not prove transport confidentiality?
2. What does packet-level TN3270E negotiation add beyond a port-23 listener?
3. Why is the raw PCAP excluded while sanitized screenshots are retained?
4. Which Academy school should own transport remediation?
5. What evidence would be needed to move from exposure baseline to protected transport?

---
### Continue learning

**Previous:** [Lab 35 — TN3270/TSO authentication boundary](../../lab-35-tn3270-tso-authentication-exposure/)  
**Networking:** [Communications Server](https://github.com/P-dot/zos-communications-server-network-lab)  
**Academy relationships:** [Security ↔ Networking](https://github.com/P-dot/P-dot/blob/main/docs/RELATIONSHIPS.md)
