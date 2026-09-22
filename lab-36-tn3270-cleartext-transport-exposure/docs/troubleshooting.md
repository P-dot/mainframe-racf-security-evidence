# Troubleshooting

## 1. wc3270 was initially connected to a different endpoint

The first working Windows wc3270 session used the Hercules-side 3270 endpoint rather than the z/OS Communications Server TCP/23 listener.

The distinction mattered:

```text
Hercules console/3270 endpoint
        !=
z/OS Communications Server TN3270 TCP/23
```

The lab therefore identified the z/OS HOME address and validated TCP/23 separately.

## 2. Kali could not reach the z/OS ETH1 address directly

The external client showed a route toward the private network but timed out when testing z/OS TCP/23.

The Hercules host, however, successfully reached the same z/OS address and TCP/23.

This confirmed that the issue was below the TN3270 application layer.

## 3. Temporary TCP relay

A Windows `portproxy` entry was used as a controlled workaround so the test could continue without reopening the broader LCS external-connectivity investigation.

This relay is documented explicitly and was removed after validation.

## 4. PowerShell firewall cmdlet mismatch

`New-NetFirewallRule` did not accept the expected parameter in the tested Windows environment.

The lab used `netsh advfirewall` instead.

The failed command is retained as troubleshooting evidence because it documents actual host behavior.

## 5. tshark dissector error

`tshark` failed with a local dissector/plugin error unrelated to the network session.

The capture was completed with `tcpdump`, which did not depend on the failing dissector path.

## 6. TN3270 payload is not ordinary ASCII

The 3270 application stream includes EBCDIC and structured 3270 data. A packet not rendering as readable ASCII is not evidence of encryption.

The negotiation string `IBM-3278-3-E` provided a clear packet-level checkpoint.
