# Attack Scenario — Controlled UACC Exposure

## Threat Model

A mainframe attacker, insider, or over-permitted user often does not need an advanced exploit if a resource is already open through RACF configuration.

The simplest example is a dataset profile with:

```text
UACC(READ)
```

That means users who are not explicitly listed in the profile's access list might still receive read access through universal access.

## Controlled Emulation

This lab emulates that risk safely with:

```text
IBMUSER.SECLAB.PUBLIC.*
```

This is not a production resource. It is a controlled training profile.

## What the Lab Demonstrates

The lab demonstrates the security difference between:

```text
PUBLIC  -> UACC(READ)
PRIVATE -> UACC(NONE)
GRANTED -> UACC(NONE) + USER2 READ
```

The point is to learn the attack path without attacking a real system resource.

## Offensive Lesson

Open universal access is often enough to create exposure. A misconfigured profile can expose data even when no user was intentionally added to the access list.

## Defensive Lesson

A defensive RACF profile should normally be designed as:

```text
UACC(NONE)
PERMIT only required users/groups
AUDIT meaningful failures or access events
```
