# RACF PROGRAM Class Review

## Evidence

Screenshot: `evidence/screenshots/01_search_class_program_profiles.png`

Command:

```text
SEARCH CLASS(PROGRAM) MASK(*)
```

Captured visible profile names include:

```text
BPX$INIT
BPX$EV003
BPXOLVD
BPXOV
BPXPLPKA
BPX...
RLOGIND
```

## Interpretation

The environment does have visible `PROGRAM` class profiles. This is important because the `PROGRAM` class can be used to control execution of named programs and to support program-control models.

This does not, by itself, prove that the environment is hardened. It only proves that the class contains profiles visible to the reviewing user.

## Finding

```text
RACF PROGRAM class profiles are present, including BPX-related entries.
```
