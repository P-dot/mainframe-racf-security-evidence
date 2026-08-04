# Risk Analysis

| Area | Risk Before | Lab Action | Residual Risk |
|---|---|---|---|
| HZSPROC | Health Checker unavailable if inactive | Started and verified | Must remain operational after IPL or be started as required |
| UNIXPRIV | UNIX privilege class might not enforce | Verified active and `UACC(NONE)` profiles | Further UNIXPRIV granularity can be reviewed later |
| TEMPDSN | Temporary dataset class not active | Activated `TEMPDSN` | Requires operational observation during normal workload |
| OPERCMDS | Operator command class not active | Enabled with guarded profile and RACLIST | Broad `MVS.**` with WARNING is not final production design |
| Sensitive Resources | High-risk resources remain flagged | Not touched | Must be handled in a separate controlled hardening lab |

## Key Security Point

Activating a RACF class improves enforcement coverage, but activating sensitive classes without policy design can disrupt operations. `OPERCMDS` therefore requires a staged approach.
