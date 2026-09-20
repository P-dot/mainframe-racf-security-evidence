# Security Analysis

## Main conclusion

The tested system exposes a real authenticated FTP-to-JES ingress path. A controlled RACF identity could authenticate to FTP, enter JES mode, and hand a JCL stream to the JES internal reader.

That capability is security-relevant because it crosses a network-facing service boundary into the batch workload subsystem. It must therefore be evaluated together with identity controls, JES policy, JCL validation, resource authorization, and monitoring.

## Important correction in evidence interpretation

Two downstream observations must remain separate:

- `ISF024I ... NOT AUTHORIZED TO SDSF, NO GROUP ASSIGNMENT` is an SDSF authorization result.
- `JCL ERROR 312` / `IEF650I UNIDENTIFIED OPERATION FIELD` is a JCL-processing result.

Neither should be relabeled as a RACF/JES submission denial without additional evidence.

## Why the result matters

A superficial test could stop after FTP returns a JES job number and claim "code execution". That would overstate what was demonstrated.

The defensible statement is narrower and stronger:

```text
authenticated FTP session
    -> JES mode
    -> internal-reader handoff
    -> JES job identity observed
```

Execution and downstream authorization remain separate validation targets.

## Security controls to investigate next

The next phase should identify which controls govern the harmless test job after syntactic correctness is established. Depending on installation configuration, relevant areas can include:

- RACF user/group state;
- JES submission and job ownership rules;
- active JES-related general-resource classes/profiles;
- dataset/resource authority used by the job;
- SDSF group/profile mapping as a separate operator/view boundary;
- audit evidence that records the tested path.

Exact profiles must be derived from the actual z/OS/JES/RACF configuration rather than assumed.

## Control discipline

Do not grant `SPECIAL`, `OPERATIONS`, UID(0), broad JES authority, or unrelated dataset/network authority merely to obtain a successful result. Any positive test should use the minimum justified change, followed by rollback and a repeat test.
