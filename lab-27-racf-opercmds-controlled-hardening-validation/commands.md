# Commands — explanation and purpose

## `RLIST OPERCMDS MVS.** ALL`
**What it is:** RACF inquiry of an existing resource profile.  
**Why used:** establish and later verify the generic OPERCMDS profile state.  
**Observed:** BEFORE `WARNING YES`; AFTER `WARNING NO`.

## `SETROPTS LIST`
**What it is:** read-only listing of global RACF options.  
**Why used:** establish the RACF class/configuration baseline before the change.

## `/D A,L`
**What it is:** read-only MVS DISPLAY activity command issued through SDSF.  
**Why used:** functional regression control.  
**Result:** allowed before and after.

## `/D IPLINFO`
**What it is:** read-only display of IPL information.  
**Why used:** second functional regression control.  
**Result:** allowed before and after.

## `/D OPDATA`
**What it is:** read-only display of operator data.  
**Why used:** third functional regression control.  
**Result:** allowed before and after.

## `/SETPROG APF,DISPLAY`
**What it is:** SETPROG operation requesting APF display information.  
**Why used:** negative security control.  
**Result before and after:** `IEE345I SETPROG AUTHORITY INVALID, FAILED BY SECURITY`.

## `RALTER OPERCMDS MVS.** NOWARNING`
**What it is:** modifies the existing generic RACF resource profile.  
**Why used:** remove warning mode without changing UACC or adding permissions.  
**Result:** RACF warned that RACLISTed profiles required refresh.

## `SETROPTS RACLIST(OPERCMDS) REFRESH`
**What it is:** refreshes RACLISTed information for OPERCMDS.  
**Why used:** make the updated profile information effective after the RACF warning.  
**Result:** no visible error; state was subsequently verified with RLIST.

## Rollback — not executed
```text
RALTER OPERCMDS MVS.** WARNING
SETROPTS RACLIST(OPERCMDS) REFRESH
```
Restores the original warning-mode state if a later validated regression requires reversal.
