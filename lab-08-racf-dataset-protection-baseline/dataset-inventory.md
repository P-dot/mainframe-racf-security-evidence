# Dataset Inventory

| Dataset | Command | Result | Security meaning |
|---|---|---|---|
| `ADCD.Z111S.PARMLIB` | `LISTDSD DA('ADCD.Z111S.PARMLIB') ALL` | No RACF description found | Sensitive configuration dataset lacks visible profile in evidence. |
| `ADCD.Z111S.PROCLIB` | `LISTDSD DA('ADCD.Z111S.PROCLIB') ALL` | No RACF description found | Started task/job procedure library lacks visible profile in evidence. |
| `ADCD.Z111S.LINKLIB` | `LISTDSD DA('ADCD.Z111S.LINKLIB') ALL` | No RACF description found | Executable library lacks visible profile in evidence. |
| `ADCD.Z111S.VTAMLIB` | `LISTDSD DA('ADCD.Z111S.VTAMLIB') ALL` | No RACF description found | VTAM/network library lacks visible profile in evidence. |
| `SYS1.PARMLIB` | `LISTDSD DA('SYS1.PARMLIB') ALL` | No RACF description found | Core system parameter library lacks visible profile in evidence. |
| `SYS1.PROCLIB` | `LISTDSD DA('SYS1.PROCLIB') ALL` | No RACF description found | Core procedure library lacks visible profile in evidence. |
| `SYS1.LINKLIB` | `LISTDSD DA('SYS1.LINKLIB') ALL` | No RACF description found | Core executable system library lacks visible profile in evidence. |
| `SYS1.LPALIB` | `LISTDSD DA('SYS1.LPALIB') ALL` | No RACF description found | LPA library lacks visible profile in evidence. |
| `SYS1.NUCLEUS` | `LISTDSD DA('SYS1.NUCLEUS') ALL` | No RACF description found | Core system dataset lacks visible profile in evidence. |
