# Part 2 Security Analysis

## Security interpretation

The primary lesson is control-plane separation.

```text
RACF identity
    |
    +--> TSO/E command authorization
    |
    +--> FTP authentication
    |
    +--> FTP JES application capability
    |
    +--> JES workload ingress
    |
    +--> batch execution identity
```

A denial at one branch is not automatically inherited by another branch.

## What was proven

- H7USER cannot invoke TSO/E `SUBMIT` in the observed configuration.
- H7USER can authenticate to the local FTP server.
- The FTP server exposes JES interface level 1.
- The FTP/JES path can submit syntactically valid JCL to JES.
- JES assigns the submitted job to H7USER.
- A harmless `IEFBR14` step can execute successfully through that path.
- No broad H7USER RACF administrative attribute explains the execution.
- The observed SURROGAT profile does not explain the job identity.

## What was not proven

- No RACF or JES2 product vulnerability was established.
- No privilege escalation was demonstrated.
- No execution under a different privileged identity was demonstrated.
- No arbitrary protected-resource access was demonstrated.
- The exact SAF decision sequence involving `JESINPUT` / `JESJOBS` for the tested FTP/JES level-1 path was not fully isolated.

## Hardening implication

A security review must evaluate each workload-ingress channel directly. Restricting TSO/E `SUBMIT` alone is not evidence that every other route into JES is equivalently restricted.

The FTP JES interface, JES-related RACF classes, internal-reader configuration, service exposure, and audit trail should be assessed together.
