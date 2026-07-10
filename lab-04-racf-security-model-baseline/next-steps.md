# Next steps

## Safe next lab candidates

1. Targeted STARTED class review

```text
SEARCH CLASS(STARTED) MASK(HZSPROC)
RLIST STARTED HZSPROC ALL
RLIST STARTED HZSPROC.* ALL
RLIST STARTED TCPIP* ALL
RLIST STARTED SDSF* ALL
```

2. Privileged user review

```text
SEARCH CLASS(USER) MASK(*)
LISTUSER <userid>
```

3. Dataset profile design exercise on a test dataset only

Create or use a non-system test high-level qualifier, then safely test UACC, PERMIT, WARNING, and AUDIT behavior without touching ADCD, SYS1, PROCLIB, PARMLIB, or APF libraries.

## Do not do yet

Do not run these on the live ADCD system without a rollback plan:

```text
SETROPTS PROTECTALL
ADDSD 'ADCD.Z111S.*' UACC(NONE)
PERMIT ADCD.Z111S.* CLASS(DATASET) ID(*) ACCESS(NONE)
ALTUSER IBMUSER NOOPERATIONS NOSPECIAL
```
