# Lab 32 — RACF Controlled Cryptographic Delegation & Key Ring Validation

## Objective

Demonstrate a controlled, least-privilege RACF authorization lifecycle for selected `RACDCERT` key-ring operations using the non-privileged test identity `H7USER`.

The lab validates effective authority rather than assuming that a successful administrative change is immediately effective.

## Scope

The lab covers only a synthetic key ring (`LAB32RING`). No certificate or private key is created, imported, exported, connected, or published.

The tested controls are:

- `IRR.DIGTCERT.LISTRING`
- `IRR.DIGTCERT.ADDRING`
- `IRR.DIGTCERT.DELRING`
- `FACILITY` RACLIST refresh behavior

## Starting point

Lab 31 established that `H7USER` could not issue the selected `RACDCERT` operations. `IRR.DIGTCERT.LISTRING` existed with `UACC(NONE)`, while the lab-specific `ADDRING` and `DELRING` control profiles used here did not exist.

## Execution flow

```text
DENIED
  |
  v
minimal function-specific authorization
  |
  v
FACILITY RACLIST refresh
  |
  v
ALLOWED
  |
  v
create and verify LAB32RING
  |
  v
separate DELRING authorization boundary
  |
  v
delete and verify LAB32RING
  |
  v
remove temporary permissions
  |
  v
observe stale RACLIST authority before refresh
  |
  v
FACILITY refresh
  |
  v
DENIED again
  |
  v
remove temporary FACILITY profiles
  |
  v
verify structural rollback
```

## Key results

### 1. Minimum read delegation

`READ` access to `IRR.DIGTCERT.LISTRING` allowed `H7USER` to issue its own `LISTRING` request. The result changed from command authorization denial to:

```text
IRRD115I User H7USER has no rings.
```

This demonstrated that the request reached key-ring lookup rather than being rejected at the RACDCERT authorization boundary.

### 2. ADDRING remained independently protected

Before ADDRING authorization, an attempted ring creation returned:

```text
IRRD101I You are not authorized to issue the RACDCERT command.
```

A discrete `IRR.DIGTCERT.ADDRING` FACILITY profile was then created with `UACC(NONE)`, and `H7USER` received only `UPDATE` access for the test.

After the FACILITY RACLIST refresh, `H7USER` created `LAB32RING`. Independent verification showed:

```text
Digital ring information for user H7USER:

Ring:
     >LAB32RING<

*** No certificates connected ***
```

### 3. DELRING was a separate privilege boundary

Having ADDRING authority did not permit ring deletion. The first DELRING attempt returned `IRRD101I`.

A separate `IRR.DIGTCERT.DELRING` profile with `UACC(NONE)` and temporary `UPDATE` access for `H7USER` was therefore used. After refresh, deletion completed and `LISTRING(*)` returned that H7USER had no rings.

### 4. RACLIST caching was functionally demonstrated

The temporary ACL entries were removed from the RACF database. Before refreshing the RACLISTed FACILITY class, `H7USER` could still issue `LISTRING` and reached the `IRRD115I` result.

After:

```text
SETROPTS RACLIST(FACILITY) REFRESH
```

the same request returned:

```text
IRRD101I You are not authorized to issue the RACDCERT command.
```

This is direct functional evidence that a RACLISTed class can continue to use cached authorization state until it is refreshed.

## Rollback

Rollback was completed at three levels.

**Object:** `LAB32RING` was deleted and absence was functionally verified.

**Authorization:** all temporary H7USER entries for LISTRING, ADDRING, and DELRING were removed; FACILITY was refreshed; the original RACDCERT denial was restored.

**Structure:** the temporary `IRR.DIGTCERT.ADDRING` and `IRR.DIGTCERT.DELRING` profiles were deleted and FACILITY was refreshed. Final `RLIST` checks returned `ICH13003I ... NOT FOUND` for both profiles.

`IRR.DIGTCERT.LISTRING` was not deleted because it existed before Lab 32.

## Security interpretation

The lab demonstrates that RACDCERT administrative capabilities can be separated by function rather than solved with broad RACF privilege. The controlled identity did not receive `SPECIAL`, `OPERATIONS`, UID(0), wildcard cryptographic administration, access to real service key rings, or private-key capabilities.

It also demonstrates why RACF change validation must include RACLIST state: database state and effective runtime authorization can temporarily differ.

## Result

**PASS — controlled RACDCERT delegation, functional validation, and full rollback demonstrated.**

No certificate or private key was created. No permanent Lab 32 key ring, H7USER cryptographic permission, or temporary ADDRING/DELRING FACILITY profile remains.

## Ecosystem relationship

```text
Communications Server Lab 09
certificate / key-ring inventory
          |
          v
RACF Lab 31
cryptographic authorization baseline
          |
          v
RACF Lab 32
controlled RACDCERT delegation
          |
          v
RACF Lab 33 (planned)
controlled synthetic certificate + key-ring lifecycle
          |
          v
Communications Server
PAGENT / TTLSRule / AT-TLS validation
```

Lab 32 owns the RACF/SAF authorization proof. It does not configure TCP/IP, Policy Agent, AT-TLS rules, or a network service.

## Evidence

The `evidence/` directory preserves the captured command sequence and rollback validation. Evidence should be reviewed for publication safety before being pushed to a public repository.
