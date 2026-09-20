# Architecture and Trust Boundaries

## Tested path

```text
z/OS TCP/IP
    |
    +-- TCP/21
         |
         v
       FTPD1
         |
         +-- RACF authentication --> H7USER
         |
         +-- FTP JES interface (JESINTERFACELEVEL=1)
                  |
                  +-- SITE FILE=JES
                  |
                  +-- STOR controlled JCL
                           |
                           v
                    JES internal reader
                           |
                           v
                    JES input / conversion
                           |
                +----------+----------+
                |                     |
                v                     v
          JCL validity          security controls
                |                     |
                v                     v
           execution path       RACF/SAF/JES policy

SDSF is a separate observation/control surface used to inspect or operate on JES data;
its authorization result must not be mistaken for the submission result itself.
```

## Trust transitions

The lab separates seven transitions:

1. Service exposure.
2. RACF-backed authentication to FTP.
3. FTP application capability (`SITE FILE=JES`).
4. Internal-reader handoff.
5. JES/JCL acceptance and conversion.
6. Downstream resource and execution authority.
7. SDSF visibility/operational authority.

A successful result at one boundary is not evidence that the next boundary is authorized.
