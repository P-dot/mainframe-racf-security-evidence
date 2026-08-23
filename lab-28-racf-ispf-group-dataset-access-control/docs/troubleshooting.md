# Troubleshooting — Generic DATASET Naming

## Symptom

Attempts to display/add:

```text
RACFL28.**
```

returned:

```text
IKJ56702I INVALID DATASET NAME
```

## Investigation

The RACF System Security Options display showed two relevant facts:

```text
GENERIC PROFILE CLASSES includes DATASET
ENHANCED GENERIC NAMING IS NOT IN EFFECT
```

Therefore generic DATASET protection was available, but the attempted enhanced `**` naming syntax did not match the system's active naming mode.

## Resolution

The lab did not enable EGN globally. The generic profile was adapted to:

```text
RACFL28.*
TYPE=GENERIC
```

RACF then returned:

```text
PROFILE ADDED
```

## Learning point

Do not treat a syntax rejection as evidence that generic DATASET profiles are unavailable. Check the installation's RACF generic-naming options first, and avoid changing global security semantics solely to make a lab example fit.
