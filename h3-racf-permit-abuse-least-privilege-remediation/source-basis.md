# Source Basis

This lab is based on the user's H3 evidence document and the previously established RACF sandbox sequence.

## User evidence

- Source Word document: `evidence/source-documents/H3_source.docx`
- Extracted screenshots: `evidence/screenshots/`
- Contact sheet: `evidence/h3_contact_sheet.jpg`

## Technical basis

The lab builds on standard RACF DATASET profile concepts:

- profiles protect resources;
- `UACC` defines universal/default access;
- `PERMIT` defines explicit access list entries;
- security review must include both universal access and explicit access list entries.

## Environment boundary

The lab was performed in an ADCD z/OS 1.11 laboratory system on Hercules/Windows and is intentionally restricted to `IBMUSER.SECLAB.*`.
