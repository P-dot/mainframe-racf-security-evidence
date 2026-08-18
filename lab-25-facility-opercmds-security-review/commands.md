# Lab 25 — Commands

## FACILITY inventory

SEARCH CLASS(FACILITY) MASK(*)

## STGADMIN discovery

SEARCH CLASS(FACILITY) FILTER(STGADMIN.**)

RLIST FACILITY STGADMIN.* ALL
RLIST FACILITY STGADMIN.IGD.* ALL

## BPX discovery

SEARCH CLASS(FACILITY) FILTER(BPX.**)

RLIST FACILITY BPX.SUPERUSER ALL
RLIST FACILITY BPX.SERVER ALL
RLIST FACILITY BPX.FILEATTR.APF ALL
RLIST FACILITY BPX.FILEATTR.PROGCTL ALL

## Troubleshooting evidence

The following exploratory queries did not return the intended MVSADMIN
profile inventory and are retained as troubleshooting evidence:

RLIST FACILITY MVSADMIN.* ALL
SEARCH CLASS(FACILITY) MASK(MVSADMIN*)

No configuration-changing RACF commands were executed.
