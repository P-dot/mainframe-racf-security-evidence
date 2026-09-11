# Planned Communications Server Handoff

## Retained RACF objects
```text
Owner:       H7USER
Certificate: LAB33CERT
Key ring:    LAB33RING
Usage:       PERSONAL
Default:     NO
```

## Planned next chain
```text
LAB33CERT + LAB33RING
        |
service identity / SAF review
        |
Policy Agent
        |
TTLSRule
        |
controlled AT-TLS target
        |
TLS and observability validation
```

Before transport enforcement, verify the exact z/OS V1R11 service/key-ring access requirements and rollback path. Do not export the private key. Do not claim AT-TLS is active merely because the RACF cryptographic objects exist.
