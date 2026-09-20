# Evidence Manifest

| File | Evidence |
|---|---|
| `01-listuser-h7user.png` | H7USER RACF baseline |
| `02-listuser-user2.png` | USER2 exists in RACF |
| `03-h7nousr-not-defined.png` | H7NOUSR does not exist in RACF |
| `04-tn3270-port23-listen.png` | TN3270 runtime listener on TCP/23 |
| `05-user2-tso-not-authorized.png` | Existing USER2 receives IKJ56420I |
| `06-h7nousr-tso-not-authorized.png` | Nonexistent H7NOUSR receives IKJ56420I |
| `07-h7user-password-prompt.png` | TSO-enabled H7USER reaches password-entry state |
| `08-h7user-wrong-password-response.png` | One wrong password produces IKJ56421I / IKJ56429A |
| `09-h7user-login-success-sanitized.png` | Later H7USER logon succeeds; ADCD sample credential table removed |

## Publication safety

No password entered by the user is retained in this evidence set. The final successful-logon screenshot is intentionally sanitized because the original ADCD banner displayed sample/default credential material unrelated to the lab objective.
