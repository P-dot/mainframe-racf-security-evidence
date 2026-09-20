# Evidence Screenshots

Copy only the screenshots needed to establish the evidence chain. Preserve originals privately; publish sanitized copies only.

Recommended order:

1. TCP/IP `PORTLIST` showing FTP-related configuration.
2. `NETSTAT CONN` showing FTPD1 on TCP/21 in LISTEN state.
3. FTPD1 address-space/runtime mapping.
4. TCP/IP profile `AUTOLOG` / `PORT` evidence.
5. FTP.DATA contextual evidence if useful.
6. Successful H7USER FTP authentication.
7. FTP `STAT` showing `JESINTERFACELEVEL=1`.
8. `SITE FILE=JES` accepted.
9. JES-oriented FTP interaction.
10. FTPJES1 JCL member as actually submitted.
11. `STOR` / internal-reader response and JES job identifier.
12. SDSF `ISF024I ... NOT AUTHORIZED TO SDSF, NO GROUP ASSIGNMENT` evidence, explicitly labeled as SDSF authorization.
13. JES/JCL diagnostics showing `JCL ERROR 312` / `IEF650I UNIDENTIFIED OPERATION FIELD`, if present in the captured run.

## Sanitization

Before publication, remove or mask unnecessary:

- passwords and credentials;
- private/non-loopback IP addresses;
- MAC addresses;
- adapter/interface identifiers;
- host-side paths and usernames;
- terminal/session identifiers;
- unrelated certificate or infrastructure identifiers.

Do not redact the technical messages required to support the lab conclusion.
