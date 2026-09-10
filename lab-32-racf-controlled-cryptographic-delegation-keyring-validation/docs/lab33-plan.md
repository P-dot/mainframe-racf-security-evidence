# Planned Lab 33 — Controlled Certificate and Key Ring Lifecycle

## Objective

Build on Labs 31–32 by creating a fully synthetic RACF cryptographic identity that can later be consumed by a controlled Communications Server AT-TLS integration lab.

## Planned scope

- select a lab-only RACF owner rather than modifying a critical service identity by default;
- create a dedicated synthetic key ring;
- create or register a lab-only certificate using the capabilities available on the z/OS V1R11 environment;
- connect the certificate to the dedicated key ring;
- verify ownership, label, trust state, ring membership, and private-key presence only where safe and required;
- preserve certificate metadata without publishing private key material;
- document exact RACF FACILITY controls and effective authority required;
- implement and verify complete rollback where compatible with the future AT-TLS handoff.

## Explicit exclusions

Lab 33 will not configure PAGENT, TTLSRule, TCP/IP profiles, or production-like network services. Those remain Communications Server responsibilities.

No real private key, internal production certificate, private CA material, or sensitive certificate metadata will be published.

## Integration handoff

```text
RACF Lab 33
synthetic certificate + dedicated key ring
        |
        v
Communications Server
Policy Agent + TTLSRule
        |
        v
controlled AT-TLS service
        |
        v
TLS validation / observability
```
