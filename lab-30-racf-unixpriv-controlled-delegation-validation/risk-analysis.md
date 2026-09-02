# Risk Analysis — Lab 30

## Risk addressed
Multiple or unnecessary UID(0) identities increase the blast radius of UNIX superuser authority. A more granular design is to authorize only the required UNIX function through RACF `UNIXPRIV` profiles where supported.

## Control demonstrated
`SUPERUSER.FILESYS.CHANGEPERMS` was used as a controlled example. H7USER received only `READ` to this profile for the duration of the test.

## Why the test is strong
The experiment kept the user, target file, and requested permission change stable. The meaningful security variable was the UNIXPRIV authorization:

`NONE → READ → removed`

The observed behavior followed the authorization state:

`DENIED → ALLOWED → DENIED`

## Residual considerations
- A UNIXPRIV authorization is still privileged and should be granted only to identities with a documented operational need.
- RACLISTed class changes must be managed carefully so cached authorization state matches the RACF database.
- UID/GID allocation must be governed to avoid accidental identity collisions.
- Production designs should use dedicated groups/roles where appropriate instead of accumulating direct user permits.
- Technical UID(0) identities in an ADCD/training image should be assessed individually; their presence alone does not prove a production vulnerability.
