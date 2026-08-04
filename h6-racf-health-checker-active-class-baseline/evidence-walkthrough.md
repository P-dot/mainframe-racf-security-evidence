# Evidence Walkthrough

## Page-level evidence from RACF.docx

- Page 1: `HZSPROC NOT ACTIVE`, followed by `HZSPROC` startup, `D A,HZSPROC` showing active address space, IBMRACF checks run, `RACF_UNIXPRIV_ACTIVE` successful and `RACF_TEMPDSN_ACTIVE` still exception.
- Page 2: `RACF_OPERCMDS_ACTIVE` exception and Health Checker log showing `TEMPDSN` and `OPERCMDS` not active while `RACF_SENSITIVE_RESOURCES` remains high-risk and out of scope.
- Pages 2-4: `RLIST UNIXPRIV SUPERUSER.FILESYS` and `SUPERUSER.FILESYS.CHOWN`, showing `UACC(NONE)`.
- Pages 5-7: `SETROPTS CLASSACT(TEMPDSN)` followed by `SETROPTS LIST`, confirming RACF active/generic/raclist class context.
- Page 8: `SEARCH CLASS(OPERCMDS) MASK(*)` showing existing `MVS.SET.PROG` and `MVS.SETPROG`, then OPERCMDS setup and the RACF warning that the class must be RACLISTed before authorization checking can occur.
- Page 9: Final CK evidence showing `RACF_UNIXPRIV_ACTIVE`, `RACF_TEMPDSN_ACTIVE`, and `RACF_OPERCMDS_ACTIVE` as `SUCCESSFUL`.
- Page 10: Health Checker log still shows `RACF_SENSITIVE_RESOURCES` as exception, confirming that this lab did not attempt broad sensitive-resource remediation.

## Image evidence

The extracted screenshots are stored in:

```text
evidence/screenshots/
```

A contact sheet is available at:

```text
evidence/h6_contact_sheet.jpg
```
