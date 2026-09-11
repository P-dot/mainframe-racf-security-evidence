# Lab 33 — RACF Controlled Certificate and Key Ring Lifecycle

## Objective
Create and validate a synthetic RACF-managed certificate and dedicated key ring under a controlled least-privilege authorization model, then remove temporary administrative authority while retaining the cryptographic objects for a future Communications Server handoff.

## Scope
Controlled identity: `H7USER`. Synthetic certificate: `LAB33CERT`. Dedicated ring: `LAB33RING`. Synthetic subject CN: `RACF Lab 33 Synthetic Server`.

No production service identity, private-key export, TCP/IP profile, Policy Agent policy, TTLSRule, or live AT-TLS service is configured here.

## Baseline
`H7USER` initially had no certificate information and no rings. `IRR.DIGTCERT.GENCERT`, `ADD`, `GENREQ`, `ADDRING`, `CONNECT`, `DELETE`, and `DELRING` were not defined. `IRR.DIGTCERT.LIST` and `LISTRING` were pre-existing controls.

## Executed flow

### Certificate generation
`IRR.DIGTCERT.GENCERT` was defined with `UACC(NONE)` and H7USER received `CONTROL`. The first GENCERT attempt was denied with `IRRD101I`.

Investigation showed additional operation-specific authorization was required in the tested z/OS V1R11 path. `IRR.DIGTCERT.ADD` and `IRR.DIGTCERT.GENREQ` were defined with `UACC(NONE)` and H7USER received `UPDATE`. After FACILITY RACLIST refresh, GENCERT completed.

`LAB33CERT` was verified with status `TRUST`, a synthetic subject/issuer, a `Non-ICSF` private key of 2048 bits, and initially no ring association. No private key was exported.

### Key-ring creation
`IRR.DIGTCERT.ADDRING` was defined with `UACC(NONE)` and H7USER received `UPDATE`. `LAB33RING` was created.

H7USER's subsequent `LISTRING` was denied, proving ADDRING authority did not imply query authority. IBMUSER independently verified that the ring existed and was empty. H7USER then received `READ` to the pre-existing `IRR.DIGTCERT.LISTRING`; after RACLIST refresh the ring could be listed.

### Certificate connection
A CONNECT attempt before CONNECT authorization was deliberately denied. `IRR.DIGTCERT.CONNECT` was then defined with `UACC(NONE)` and H7USER received `CONTROL`.

After RACLIST refresh, CONNECT succeeded. `LISTRING` verified `LAB33CERT` in `LAB33RING`, owned by `ID(H7USER)`, usage `PERSONAL`, `DEFAULT NO`.

### Authorization rollback and RACLIST behavior
Temporary H7USER access was removed from GENCERT, ADD, GENREQ, ADDRING, LISTRING, and CONNECT.

Before FACILITY RACLIST refresh, H7USER could still list the ring. After `SETROPTS RACLIST(FACILITY) REFRESH`, the same operation returned `IRRD101I`. This demonstrates the difference between RACF database ACL state and effective RACLISTed authority.

### Structural rollback
The five Lab 33-created FACILITY profiles were deleted: GENCERT, ADD, GENREQ, ADDRING, and CONNECT. After RACLIST refresh, all five `RLIST` checks returned `ICH13003I ... NOT FOUND`.

The pre-existing LIST and LISTRING profiles were preserved.

## Final retained state
Temporary administrative delegation: **removed**. Lab-created FACILITY profiles: **removed**. Effective cache: **refreshed**. H7USER RACDCERT query after refresh: **denied**.

`LAB33CERT`, its RACF-managed private key, `LAB33RING`, and the certificate-to-ring association were deliberately retained. Final IBMUSER verification confirmed status `TRUST`, 2048-bit Non-ICSF private key, ring association `LAB33RING`, usage `PERSONAL`, and `DEFAULT NO`.

## Security findings
- RACDCERT administration is separated by operation-specific controls.
- GENCERT authorization alone was insufficient in the tested path.
- ADDRING authority did not imply LISTRING authority.
- LISTRING authority did not imply CONNECT authority.
- `UACC(NONE)` maintained default deny for introduced controls.
- RACLIST refresh materially changed effective authorization after ACL removal.
- Cryptographic-object persistence is independent of temporary administrative delegation.
- Silent RACDCERT completion must be verified by querying resulting state.

## Cross-repository handoff
RACF owns certificate/key-ring identity, RACDCERT authorization, `IRR.DIGTCERT.*`, effective authority, and security evidence. Communications Server owns Policy Agent, TTLSRule, TCP/IP policy, AT-TLS enforcement, and transport validation.

Planned chain:

```text
RACF Lab 31 -> RACF Lab 32 -> RACF Lab 33
                                  |
                         LAB33CERT + LAB33RING
                                  |
                                  v
                    Communications Server [planned]
                    Policy Agent / TTLSRule / AT-TLS
```

No end-to-end TLS implementation is claimed by this lab.

## Result
**PASS** — controlled certificate/key-ring lifecycle, negative and positive authorization testing, RACLIST behavior, structural rollback, and a retained synthetic cryptographic identity were validated.
