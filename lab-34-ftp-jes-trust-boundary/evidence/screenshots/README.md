# Lab 34 — Screenshot Evidence Index

These screenshots are extracted from the controlled Lab 34 run and retained as publication-safe evidence.

The selected images avoid non-loopback host IP addresses, MAC addresses, host adapter identifiers, credentials, private keys, and local host filesystem paths. `127.0.0.1`, `0.0.0.0`, the synthetic test identity `H7USER`, and laboratory JES identifiers are retained because they are directly relevant to the technical evidence.

| File | Evidence | Interpretation |
|---|---|---|
| `01-netstat-portlist.png` | z/OS Communications Server port inventory | FTP port 21 is present in the configured service inventory. |
| `02-netstat-ftp-listener.png` | `NETSTAT CONN` runtime state | `FTPD1` is listening on TCP/21. |
| `03-ftpd1-address-space.png` | active-address-space display | Correlates the running `FTPD1` service with the laboratory runtime. |
| `04-ftp-local-connect.png` | FTP client connection to loopback TCP/21 | Establishes the controlled local client path to the FTP server. |
| `05-h7user-ftp-authentication.png` | successful FTP login as `H7USER` | Proves FTP authentication for the controlled RACF identity; it does not prove downstream JES authority. |
| `06-ftp-stat-jesinterfacelevel1.png` | FTP `STAT` output | Shows effective runtime `JESINTERFACELEVEL is 1`. |
| `07-site-file-jes-accepted.png` | `SITE FILE=JES` response | Proves the FTP session entered JES mode. |
| `08-jes-mode-list.png` | JES-mode `LIST` | Shows JES-oriented FTP interaction after the mode switch. |
| `09-ftpjes1-controlled-jcl.png` | controlled `IEFBR14` JCL member | Documents the harmless workload intended for the test. |
| `10-ftp-jes-internal-reader-job07414.png` | FTP `PUT` result | Shows handoff to the JES internal reader and assignment of `JOB07414`. This proves JES ingress, not successful execution. |
| `11-sdsf-isf024i-denial.png` | `ISF024I` | SDSF authorization denial for `H7USER`; this is not evidence that FTP submission was denied. |
| `12-job07414-jcl-error312.png` | JES2 job log | Shows `JOB07414` was not run because of `JCL ERROR 312`. |
| `13-ief650i-unidentified-operation-field.png` | JCL diagnostic | Shows `IEF650I UNIDENTIFIED OPERATION FIELD`; this is a JCL-processing result, not a RACF denial. |

## Evidence boundary

The screenshots support the following chain:

```text
FTP service active
    -> H7USER authenticated
    -> JES interface enabled
    -> SITE FILE=JES accepted
    -> JCL stream sent to JES internal reader
    -> JOB07414 assigned
    -> JCL processing failed
    -> SDSF access was independently denied
```

They do **not** prove successful workload execution or a RACF/JES execution-boundary denial. Those remain separate validation targets for the next phase.
