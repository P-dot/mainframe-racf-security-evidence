# ADCD Limitations

This audit was performed on an ADCD educational environment. Several findings are expected in a lab distribution and should not be interpreted as a failure of IBM z/OS.

## Important limitations

- ADCD is designed for learning, demonstration and product exploration.
- Default users and groups may be intentionally permissive.
- Many product libraries may not be protected as they would be in production.
- SMF, zSecure, DSMON and full compliance reporting may not be configured.
- The environment is small, single-user/lab-oriented and not representative of enterprise separation of duties.

## How to present the work

Correct presentation:

```text
This is an educational audit of an ADCD/Hercules lab system. The work demonstrates methodology, evidence collection and risk reasoning.
```

Incorrect presentation:

```text
This is a certified production RACF audit.
```

## Professional value

The portfolio value is not that the lab system is hardened. The value is that the audit identifies weaknesses, explains risk, avoids unsafe remediation, and proposes a controlled hardening roadmap.
