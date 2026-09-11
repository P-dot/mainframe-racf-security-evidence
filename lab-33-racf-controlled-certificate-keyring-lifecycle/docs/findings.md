# Lab 33 — Findings

## Effective-authority result
The lab demonstrated a sequence of independent RACDCERT security boundaries: certificate generation, certificate-related functions, key-ring creation, key-ring query, and certificate-to-ring connection.

The access levels below are the levels actually exercised in this environment, not a universal authority matrix:

| Function | FACILITY resource | H7USER access exercised |
|---|---|---|
| GENCERT | `IRR.DIGTCERT.GENCERT` | CONTROL |
| ADD | `IRR.DIGTCERT.ADD` | UPDATE |
| GENREQ | `IRR.DIGTCERT.GENREQ` | UPDATE |
| ADDRING | `IRR.DIGTCERT.ADDRING` | UPDATE |
| LISTRING | `IRR.DIGTCERT.LISTRING` | READ |
| CONNECT | `IRR.DIGTCERT.CONNECT` | CONTROL |

## Strong evidence
The workflow deliberately captured DENIED -> delegated -> ALLOWED transitions. It also reproduced the RACLIST cache effect: ACL deletion did not immediately remove effective authority; FACILITY RACLIST refresh did.

## Retained cryptographic state
`LAB33CERT` remains TRUSTed with a 2048-bit Non-ICSF private key and is connected to `LAB33RING`. The ring reports owner `ID(H7USER)`, usage `PERSONAL`, default `NO`.

No private key material was exported.

## Rollback model
This is a controlled retained-state rollback. Temporary H7USER administrative ACLs and Lab-created FACILITY profiles were removed, but the synthetic certificate, private key, ring, and association were retained for a future Communications Server integration exercise.

## Ownership boundary
The RACF repository owns RACF/SAF authorization and cryptographic identity. The Communications Server repository must own any future Policy Agent, TTLSRule, TCP/IP policy, AT-TLS activation, handshake testing, and transport observability.

The current evidence does not demonstrate an active network TLS path.
