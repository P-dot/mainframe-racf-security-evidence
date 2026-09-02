# Evidence Manifest — Lab 30

This package contains **45 unique embedded screenshots** extracted from the DOCX evidence files supplied during the execution of Lab 30. Duplicate embedded images were removed by SHA-256 content hash during packaging.

The evidence set covers the chronological lab flow:
- H7GRP baseline and OMVS/GID preparation.
- Candidate UNIXMAP checks for GID/UID 1000.
- H7USER OMVS segment preparation and verification.
- Initial OMVS failure caused by missing HOME directory.
- Creation and validation of `/u/h7user`.
- Successful H7USER OMVS entry and identity validation.
- UNIXPRIV profile baseline.
- Controlled IBMUSER-owned test file.
- BEFORE denial from H7USER.
- Minimal `READ` permit and RACLIST refresh.
- AFTER successful permission change.
- ACL rollback and refresh.
- Final denial proving effective rollback.

Screenshots are retained as execution evidence; the README and command log provide the authoritative narrative and sequence.
