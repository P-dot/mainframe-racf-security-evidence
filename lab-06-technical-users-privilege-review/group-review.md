# Group Review

## `SYS1`

`LISTGRP SYS1` shows:

- `GROUP=SYS1`
- `OWNER=IBMUSER`
- `SUPERIOR GROUP=NONE`
- multiple subgroups, including system and product-related groups
- `IBMUSER` connected with `AUTH=JOIN`
- many technical/system IDs connected with `AUTH=USE`, including `START1`, `START2`, `FTPD`, `INETD`, `BPXOINIT`, `CEA`, and others

## Meaning

`SYS1` is the central/root RACF group in this ADCD environment. A large number of system and service IDs are connected to it. This is expected in a training ADCD system, but in a hardened production system it should be reviewed carefully.

## Non-existent group checks

The following commands returned `ICH51003I NAME NOT FOUND IN RACF DATA SET`:

- `LISTGRP START1`
- `LISTGRP START2`
- `LISTGRP TCPIP`
- `LISTGRP FTPD`
- `LISTGRP WEBSRV`
- `LISTGRP OMSGRP`

This is not necessarily a problem. It confirms that these names are user IDs, not group IDs. However, `OMSGRP` appears to be a typo or incomplete group name. The visible user profiles show `OMVSGRP`, so the recommended follow-up is:

```text
LISTGRP OMVSGRP
```
