# Recruiter summary

This combined lab demonstrates a practical RACF/z/OS security review across two layers:

1. zFS backing dataset protection.
2. SAF controls in FACILITY, UNIXPRIV, SERVAUTH and STARTED.

Evidence includes TSO/ISPF and SDSF screenshots showing:

- active zFS filesystems mounted RDWR;
- missing visible RACF DATASET profiles for reviewed zFS backing datasets;
- no visible generic dataset profiles for checked CICS/DB2 dataset families;
- no visible BPX.* FACILITY profiles;
- no visible UNIXPRIV profiles;
- no visible checked EZB.* SERVAUTH profiles;
- existing `TCPIP.*` STARTED profile with UACC(NONE), but no visible STDATA in the captured output.

The lab shows hands-on ability to collect RACF evidence, interpret SAF classes, identify least-privilege gaps, and document risk without making unsafe changes to a z/OS system.
