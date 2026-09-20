# Lab 35 Findings

## F1 — TN3270 exposes the TSO logon surface on TCP/23

Runtime evidence confirms the TN3270 listener was active during the authentication tests.

## F2 — USER2 and H7NOUSR produce the same TSO response

`USER2` is a defined RACF identity while `H7NOUSR` is not defined. Both produced `IKJ56420I ... not authorized to use TSO`.

The tested response therefore does not support distinguishing those two identity states.

## F3 — A TSO-enabled identity produces a different failure class

`H7USER` reached the password stage and a single incorrect password produced `IKJ56421I PASSWORD NOT AUTHORIZED FOR USERID` followed by `IKJ56429A REENTER -`.

## F4 — The controlled identity remained usable

A subsequent correct H7USER authentication progressed into the TSO/ISPF environment.

## Final finding

The tested service exposes a limited authentication-response distinction:

```text
TSO-enabled userid + bad password
            !=
non-TSO / nonexistent userid
```

The lab does not claim complete RACF userid enumeration.
