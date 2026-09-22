# Security Analysis

## Main conclusion

Lab 36 validates that the tested TN3270 transport path carried Telnet/TN3270E negotiation without a TLS layer.

The most useful evidence is not the use of a 3270 client itself, but the packet-level visibility of the negotiation data, including `IBM-3278-3-E`.

## Security relevance

Classic TN3270 can expose application/session information to any actor with legitimate packet visibility on the relevant network path.

The practical risk depends on network placement, segmentation, interception opportunities, service exposure, monitoring and whether TLS/AT-TLS protects the service.

## What was proven

- TN3270 active during the test.
- TCP/23 listener active.
- controlled remote c3270 session reached the VTAM entry screen.
- Telnet/TN3270E negotiation was present in captured packet payload.
- `IBM-3278-3-E` was observable.
- no TLS handshake preceded the observed TN3270E negotiation.
- temporary relay and firewall changes were cleaned up.

## What was not proven

- No password was captured.
- No userid/password pair was intercepted.
- No account takeover occurred.
- No RACF authorization control was bypassed.
- No transparent MITM was performed.
- No direct capture of the z/OS ETH1 leg was made.
- No claim is made that every production TN3270 service has the same configuration.

## Defensive interpretation

This finding supports reviewing:

- secure TN3270 / TLS or AT-TLS;
- network segmentation;
- reachability restrictions;
- monitoring of interactive access paths;
- migration away from unprotected classic TN3270 where appropriate.

The Communications Server repository remains the correct location for implementation-level AT-TLS engineering.
