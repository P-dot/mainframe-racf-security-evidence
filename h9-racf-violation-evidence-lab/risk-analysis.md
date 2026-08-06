# Risk Analysis

## Risk demonstrated

A user without explicit authorization attempted to read protected data.

In a production system, repeated or unexpected `ICH408I` messages may indicate:

- privilege probing
- misconfigured access lists
- batch jobs running under the wrong identity
- application identity issues
- unauthorized dataset discovery attempts

## Defensive value

The lab demonstrates how to interpret the evidence instead of treating a denial as a generic error.

## Severity in this lab

```text
Low: controlled sandbox, no sensitive production data involved.
```

## Severity in production

```text
Medium to High: depends on the dataset, user identity, frequency and context.
```
