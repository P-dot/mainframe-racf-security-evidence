# Source-video mapping

The source video moves from the TSO/E logon panel into a user-enumeration discussion and then shows a proof-of-concept tool named `v2 TSO Brute` using a 3270 automation library.

This lab implements the underlying security question without copying the uncontrolled enumeration behavior:

```text
video concept
   |
   v
differential TSO logon responses
   |
   v
controlled three-identity matrix
   |
   v
single wrong-password validation
   |
   v
evidence-driven conclusion
```

The video is treated as a threat hypothesis, not as proof of the local system's behavior.

The local evidence showed only a limited distinction: a TSO-enabled userid with a bad password produced a different message from identities that could not use TSO, while an existing non-TSO userid and a nonexistent userid produced the same message.
