# Recommended next step

## Immediate video continuation

The next source-video section is the `v2 TSO Brute` proof of concept, followed later by Netcat on OMVS.

## Recommendation

Do not repeat a broad password-brute-force exercise. The manual Lab 35 already proves the response distinction the video relies on.

If the automation concept itself is worth reproducing, create a separate controlled Lab 36 that:

- uses only an allowlisted set of lab identities;
- performs userid classification only;
- applies a fixed delay between tests;
- does not guess passwords;
- records the observed TSO response class;
- compares automated output with the Lab 35 manual ground truth.

This follows the video closely while adding engineering value rather than merely increasing request volume.

After that, continue to the video's OMVS/Netcat section as a separate UNIX/network tooling lab.
