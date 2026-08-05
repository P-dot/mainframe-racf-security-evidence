# Risk Analysis

## Risk reduced

Using `H7USER` instead of `IBMUSER` reduces the risk of testing with excessive privileges.

## Main security benefit

Future labs can now distinguish between:

- access granted because the profile allows it
- access denied because the profile blocks it
- access accidentally bypassed because a user is too powerful

## Residual risk

The lab user now has TSO access. It must remain constrained:

- no privileged attributes
- no production dataset permissions
- no elevated group connections
- no OPERCMDS authority
- no UID(0)

## Operational note

The evidence contains setup and account-number troubleshooting. Raw working documents should not be published if they expose temporary password material.
