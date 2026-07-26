# Executive Summary

## Environment reviewed

The audit was performed on an IBM ADCD z/OS 1.11 educational environment running under Hercules. The assessment focused on RACF, OMVS, zFS, SAF general resource classes, JES2/SDSF, audit logging, and authorized code exposure.

## Overall result

The environment is appropriate for laboratory work and controlled training. It is not configured as a hardened production baseline.

## Key risk themes

1. **Privilege concentration**  
   `IBMUSER`, `START1`, `START2` and several technical service IDs have broad authority or strong operational relevance.

2. **OMVS superuser exposure**  
   Multiple technical IDs were observed with `UID(0)` or mapped to UNIX superuser identity, reducing separation of duties and accountability.

3. **Sensitive DATASET protection gaps**  
   Several sensitive system, product, zFS and authorized-code datasets returned `NO RACF DESCRIPTION FOUND` in the collected evidence.

4. **Limited visible granular SAF controls**  
   The audit did not show broad use of granular profiles for `BPX.*`, `UNIXPRIV`, `EZB.*`, `OPERCMDS`, `JESJOBS`, `JESSPOOL` or `SDSF`.

5. **Audit/logging limitations**  
   The SMF evidence showed `SYS1.MAN RECORDING NOT BEING USED` and `SMFPRM00` showed `NOACTIVE`, limiting forensic confidence in this lab configuration.

6. **Authorized code exposure**  
   APF and LINKLIST are active. PROGRAM profiles exist. Several reviewed libraries associated with APF/LINKLIST/authorized code did not show visible DATASET profiles in the captured evidence.

## Recommended remediation direction

Do not harden the ADCD system blindly. Build a controlled hardening track using test datasets, reversible profiles, documented before/after evidence, and rollback steps.

Recommended phased path:

1. Stabilize audit evidence and backups.
2. Enable and validate audit logging in a controlled way.
3. Model DATASET protection using test libraries.
4. Reduce unnecessary UID(0) patterns in a controlled lab.
5. Introduce granular SAF profiles for OMVS/TCP/IP/JES/SDSF controls.
6. Protect APF, LINKLIST and authorized-code libraries.
7. Add continuous monitoring using Health Checker style checks and zSecure-style reporting where available.
