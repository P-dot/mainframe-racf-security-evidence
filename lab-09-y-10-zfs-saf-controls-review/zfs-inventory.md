# zFS inventory observed with /D OMVS,F

`/D OMVS,F` showed several active zFS filesystems mounted in `RDWR` mode.

Observed examples:

| zFS / aggregate | Product area | Observed status | Security meaning |
|---|---:|---:|---|
| `DFH410.ZFS` | CICS TS 4.1 | Active / RDWR | CICS-related zFS filesystem. Must be protected as a sensitive backing dataset. |
| `ACD211.SACDZFS1` | ADCD / system component | Active / RDWR | ADCD-related filesystem. Requires dataset profile review. |
| `DSN910.SJVAZFS` | DB2 9.1 | Active / RDWR | DB2 Java/support zFS candidate. Requires RACF DATASET control. |
| `DAH910.ADAHZFS1` | DB2 / ADCD component | Active / RDWR | DB2/ADCD-related filesystem. Requires review. |
| `DSN910.SDSNWORF` | DB2 9.1 | Active / RDWR | DB2-related zFS. Requires review. |
| `DSN910.SDSNMQS` | DB2 9.1 | Active / RDWR | DB2-related zFS. Requires review. |
| `DSN910.SDSNJCC` | DB2 9.1 | Active / RDWR | DB2 JDBC-related zFS candidate. Requires review. |

## Key interpretation

A zFS is seen by users as a UNIX filesystem, but its storage is backed by an MVS dataset. Therefore a full review must include:

1. UNIX permissions and UID/GID model.
2. RACF DATASET profile protecting the backing dataset.
3. SAF controls for privileged UNIX functions.

This lab focuses on points 2 and 3.
