# Recruiter Summary

Performed a read-only RACF security review of z/OS UNIX / OMVS technical identities in an ADCD z/OS 1.11 environment.

Reviewed OMVS segments, UID 0 exposure, UNIXMAP U0 mappings, UNIXPRIV coverage, BPX.* FACILITY profile presence, and technical user separation for TCPIP, FTPD, WEBSRV, OMVSKERN, START1, START2 and IBMUSER.

Key findings included multiple technical IDs sharing UID 0, absence of visible UNIXPRIV and BPX.* FACILITY profiles, and reliance on UNIXMAP U0 due to AIM limitations for UID searching.

This lab demonstrates practical RACF/OMVS audit methodology, evidence collection, and risk interpretation without modifying system security definitions.
