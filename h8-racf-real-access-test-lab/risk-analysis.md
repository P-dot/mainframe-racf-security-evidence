# Risk Analysis

## UACC(READ)

Risk:

```text
Any RACF user not otherwise restricted may read data protected by the profile.
```

Impact:

```text
Possible disclosure of configuration, JCL, operational notes, application data, or technical naming patterns.
```

## UACC(NONE) without explicit permit

Risk reduction:

```text
Access is denied unless explicitly granted.
```

Impact:

```text
Supports least privilege.
```

## Explicit READ

Risk reduction:

```text
Allows only the intended user to read.
```

Impact:

```text
Demonstrates controlled access assignment.
```

## Audit readiness

Risk reduction:

```text
Failed reads can be configured for audit.
```

Limitation:

```text
RACF audit configuration is not the same as complete forensic visibility if SMF/logging is not healthy.
```
