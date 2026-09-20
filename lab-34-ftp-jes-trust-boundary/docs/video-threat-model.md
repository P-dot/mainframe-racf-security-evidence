# Source Video — Threat-Model Contribution

The source training video presents a mainframe attack-surface perspective rather than a production-hardening procedure. Relevant themes visible in the material include:

- TN3270 as a historically clear-text remote-access surface;
- FTP as a commonly exposed z/OS service;
- the FTP `SITE FILE=JES` capability as a path from a network service into JES;
- RACF credentials as an important identity boundary;
- the possibility of uploaded JCL being used to create execution paths when downstream controls permit it;
- OMVS / UNIX capabilities as another cross-domain surface.

## How this lab uses the video

The video is not treated as evidence that the local ADCD/Hercules system is exploitable in the same way. Instead it supplies a testable hypothesis:

```text
network-facing FTP
        |
        v
authenticated identity
        |
        v
SITE FILE=JES
        |
        v
JES internal reader
        |
        v
security-sensitive workload boundary
```

The laboratory then validates each transition independently using a harmless workload and preserves negative or ambiguous evidence instead of forcing the demonstration to match the video.

## Defensive value

This converts an offensive demonstration into a defensive engineering exercise:

```text
attack idea
   -> inventory exposure
   -> verify runtime capability
   -> map identity
   -> test controlled ingress
   -> classify downstream controls
   -> document evidence
   -> design least-privilege remediation
```

No reverse shell, credential attack, persistence mechanism, or destructive payload is required for the security objective of this lab.
