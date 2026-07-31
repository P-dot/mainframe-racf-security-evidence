# Attack Scenario

## Controlled weakness

The risky condition simulated in this lab is:

```text
UACC(NONE)
WARNING(YES)
```

At first glance, the profile looks closed because universal access is `NONE`.

The problem is that `WARNING` changes enforcement behavior: it is not a final blocking posture. It is useful during transition, but dangerous if forgotten.

## Why this matters

A defender might believe a resource is protected because `UACC(NONE)` appears in the profile.

A stronger RACF review checks both:

```text
UNIVERSAL ACCESS
WARNING
AUDITING
ACCESS LIST
```

The dangerous pattern is:

```text
The access policy looks restrictive,
but the profile is still in warning mode.
```

## Attacker mindset

A weak configuration does not always look like `UACC(READ)`.

Sometimes the weakness is a hidden implementation state:

```text
WARNING(YES)
```

That is why a security engineer must read the full RACF profile, not only the first line.
