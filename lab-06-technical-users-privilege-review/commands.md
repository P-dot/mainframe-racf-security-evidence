# Commands Executed

All commands were executed from ISPF option 6 / TSO Command Shell. No leading slash `/` was used because these are RACF TSO commands, not MVS operator commands.

```text
LISTUSER START1
LISTUSER START2
LISTUSER TCPIP
LISTUSER FTPD
LISTUSER WEBSRV
LISTUSER OMVSKERN
LISTUSER IBMUSER
```

Group review commands:

```text
LISTGRP SYS1
LISTGRP START1
LISTGRP START2
LISTGRP TCPIP
LISTGRP FTPD
LISTGRP WEBSRV
LISTGRP OMSGRP
```

Note: `LISTGRP OMSGRP` returned `ICH51003I NAME NOT FOUND IN RACF DATA SET`. The visible user profiles show `OMVSGRP` as the group name for some IDs, so a follow-up command should be:

```text
LISTGRP OMVSGRP
```

## Why these commands matter

`LISTUSER` shows the RACF profile of a user ID: owner, default group, global attributes, revoke status, password metadata, connected groups, and optional segments.

`LISTGRP` shows the RACF group profile: owner, superior group, subgroups, connected users, access count, and authority level such as `USE` or `JOIN`.
