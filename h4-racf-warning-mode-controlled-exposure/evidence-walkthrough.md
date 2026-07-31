# Evidence Walkthrough

## Page and screenshot interpretation

The uploaded Word evidence contains 10 rendered pages and 20 extracted screenshots.

Key observations:

1. The initial `LISTCAT` attempt on page 1 shows a not-found condition for a warning sandbox dataset name. This is useful as a precheck/typing lesson: validate the exact dataset name before assuming the dataset exists.
2. The baseline profile evidence shows `IBMUSER.SECLAB.WARNING.*` with `UNIVERSAL ACCESS: NONE` and `WARNING: NO`.
3. The warning-mode evidence shows the same profile with `WARNING: YES`.
4. The audit evidence shows `AUDITING ALL(READ)`.
5. The final remediation evidence shows `WARNING: NO` again.
6. The final sandbox comparison shows:
   - `PUBLIC` remains open with `UACC(READ)`.
   - `PRIVATE` remains closed with `UACC(NONE)`.
   - `GRANTED` remains closed with `USER2 READ`.
   - `WARNING` ends closed with `WARNING(NO)`.

## Evidence files

Screenshots:

```text
evidence/screenshots/
```

Contact sheet:

```text
evidence/h4_contact_sheet.jpg
```

Source document:

```text
evidence/source-documents/WARNING.docx
```
