# Lab 36 Findings

## F1 — TN3270 was active on classic TCP/23

Runtime evidence showed the TN3270 address space active and a TCP/23 listener.

## F2 — The tested external client did not have a direct path to the z/OS LCS address

The external Kali/WSL client could not directly reach the z/OS ETH1 address, while the Hercules host could reach the same address and TCP/23.

This is a topology/reachability limitation and is kept separate from the TN3270 transport finding.

## F3 — A temporary relay enabled controlled access without changing the application protocol

A Windows TCP relay was used as a reachability aid. It terminated one TCP connection and originated another connection to the z/OS TN3270 listener.

The capture therefore represents the client-to-relay leg, not a transparent capture of the z/OS ETH1 segment.

## F4 — TN3270E negotiation was observable in the packet stream

The packet payload exposed Telnet negotiation and the terminal identifier:

```text
IBM-3278-3-E
```

This is direct packet-level evidence that the application negotiation was not protected by TLS on the tested stream.

## F5 — No TLS handshake was observed

The observed sequence transitioned from TCP establishment directly into Telnet/TN3270E negotiation and 3270 data.

## F6 — The temporary network change was removed

The Lab 36 firewall rule and portproxy entry were deleted and the absence of the relay entry was verified.

## Final finding

The tested TN3270 path exposed protocol negotiation and application data without TLS.

The lab validates **cleartext transport exposure**, not credential recovery or account compromise.
