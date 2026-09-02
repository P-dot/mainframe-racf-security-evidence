# Troubleshooting — FSUM2384 when starting OMVS

## Symptom
H7USER had an OMVS segment containing UID, HOME, and PROGRAM, but OMVS could not start a session.

The observed failure indicated that the system could not change the current working directory to `/u/h7user`, ending with `EDC5129I No such file or directory`.

## Root cause
`HOME('/u/h7user')` in the RACF OMVS segment defines a path; it does not create the corresponding directory in the z/OS UNIX filesystem.

## Resolution
IBMUSER created the required lab directory:

```sh
mkdir /u/h7user
ls -ld /u/h7user
```

A fresh H7USER login was then used and OMVS started successfully.

## Lesson
When enabling a RACF identity for z/OS UNIX, validate both layers:
1. RACF identity metadata: UID, GID association, HOME, PROGRAM.
2. USS resources: the configured HOME path must actually exist and be usable.
