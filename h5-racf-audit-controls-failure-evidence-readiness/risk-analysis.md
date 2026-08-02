# Risk Analysis

## Risk: controls without evidence

A RACF profile can deny access, but without reliable audit capture and SMF processing, incident response teams may struggle to prove who attempted what.

## Risk: excessive audit noise

`AUDIT(ALL(READ))` can be useful temporarily, but it can generate more data than needed. For stable production controls, targeted audit settings are usually preferable.

## Risk: weak test identity model

Testing access failures with a privileged account such as `IBMUSER` is misleading. A valid low-privilege test identity is required for realistic access testing.

## Current lab risk level

Low, because all changes were limited to the sandbox namespace `IBMUSER.SECLAB.AUDIT.*`.
