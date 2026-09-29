# Part 2 Handoff

## Preconditions

Do not start Part 2 until all of the following are true:

```text
LCS host backend opens successfully
ETH1 is operational
host can reach the z/OS guest
TCP/21 or another controlled transfer path works
```

## Inputs already prepared

Windows staging already contains:

```text
netcat.c
generic.h
```

USS workspace already exists:

```text
/u/ibmuser/lab37-nc110
```

The native z/OS compiler exists:

```text
z/OS V1.11 XL C/C++
```

## Part 2 first milestone

Part 2 should stop initially at:

```text
./nc -h
```

Only after the binary is verified should the lab proceed to a harmless text exchange.

Suggested first payload:

```text
LAB37-TEST
```

## Part 2 success criterion

A successful controlled data exchange between OMVS and the lab client, with evidence explaining any EBCDIC/ASCII transformation.

No shell execution is required to close that milestone.
