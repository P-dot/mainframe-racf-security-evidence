# Source Basis

This lab is based on the RACF dataset-protection model used in z/OS Security Server RACF administration:

- dataset profiles are created with `ADDSD`
- universal access is controlled with `UACC`
- specific users or groups are authorized with `PERMIT`
- profile information is verified with `LISTDSD`
- audit controls can be adjusted with `ALTDSD`

The practical evidence comes from the uploaded Word document:

```text
evidence/source-documents/LAB01.docx
```

This lab also follows the safe-lab principle used throughout the project:

- work under `IBMUSER.SECLAB.*`
- avoid system datasets
- avoid destructive commands unless explicitly performing rollback
- preserve screenshots as primary evidence
- document failed syntax and corrections instead of hiding them
