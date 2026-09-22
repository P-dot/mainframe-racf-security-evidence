# Evidence Manifest

All publication images are sanitized. Private network data, MAC addresses, hostnames, terminal/session identifiers and local paths are pixelated where they could disclose the laboratory network.

| File | Evidence |
|---|---|
| `01-tn3270-address-space-active-sanitized.png` | TN3270 address space active |
| `02-tn3270-port23-listener.png` | TN3270 listening on classic TCP/23 |
| `03-zos-home-address-eth1-sanitized.png` | HOME address list with private address pixelated |
| `04-zos-route-table-sanitized.png` | Routing table with private network/gateway data pixelated |
| `05-host-connectivity-tcp23-sanitized.png` | Hercules-host reachability and TCP/23 validation with network data pixelated |
| `06-portproxy-configuration-sanitized.png` | Temporary controlled TCP relay with addresses pixelated |
| `07-c3270-vtam-entry-screen-sanitized.png` | Remote c3270 VTAM entry screen with network/session data pixelated |
| `08-tcpdump-session-capture-sanitized.png` | Client-side TCP capture with addressing/local path data pixelated |
| `09-tn3270e-ibm3278-negotiation-sanitized.png` | `IBM-3278-3-E` packet-level negotiation evidence with network data pixelated |
| `10-network-diagnostics-sanitized.png` | Host-side network diagnostics / adapter evidence with sensitive network identifiers pixelated |

## Raw packet capture

The raw PCAP is intentionally **not** included in the public repository.

It remains local because it contains real addressing and packet metadata not required for public proof.

## Publication rule

The public screenshots preserve the technical command/result relationship while pixelating only network/infrastructure details that are unnecessary to disclose.
