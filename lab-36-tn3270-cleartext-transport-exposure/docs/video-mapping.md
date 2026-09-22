# Source-video mapping

The source video moves from TSO authentication behavior into interception of 3270/TN3270 traffic.

Lab 36 maps that concept into a controlled defensive validation:

```text
video interception concept
        |
        v
identify real TN3270 TCP/23 path
        |
        v
establish controlled remote session
        |
        v
capture own client-side traffic
        |
        v
inspect Telnet/TN3270E negotiation
        |
        v
validate cleartext transport exposure
```

The lab intentionally does not capture credentials and does not perform an uncontrolled MITM exercise.

The video is treated as a threat hypothesis. Local packet evidence is used to determine what the actual ADCD/Hercules environment exposes.
