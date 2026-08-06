# Hardening Notes

## Correct defensive posture

Protected datasets should generally follow:

```text
UACC(NONE)
explicit access list only
AUDIT for relevant failures or sensitive resources
```

## Useful follow-up controls

- Review unexpected `ICH408I` messages.
- Correlate user, group, job/session and dataset.
- Maintain least privilege access lists.
- Avoid broad UACC access on sensitive datasets.
- Keep SMF logging healthy.

## What was not changed

This lab did not change global audit policy, production profiles or system datasets.
