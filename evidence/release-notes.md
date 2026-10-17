# Public evidence release notes

**Assessment:** 7–16 October 2026  
**Environment:** Kali Linux / Ubuntu, deliberately isolated VMware subnet  
**Dataset:** Synthetic MedTechCare training data only (confirmed by lab owner)

## What is public

- The report is a chronological write-up with embedded screenshots.
- The `scans/` folder contains seven Nmap normal-output (`-oN`) text files, copied from original scan evidence and reviewed for publication.
- The `screenshots/` folder contains fifteen cropped screenshots showing VM/terminal content.
- The MySQL handshake's transient `Salt` value is intentionally redacted from public scan text and screenshot.

## What remains private

- Unmodified original scan and screenshot captures.
- Any internal-only working drafts and publication checklists.

## Interpretation safeguards

- The `dbadmin@'%'` MySQL account was configured intentionally for an earlier MSc proof of concept. Its permissions are educational configuration evidence; no compromise is claimed.
- The `mysql-empty-password` NSE result was negative/inconclusive, not a proof that all accounts have secure passwords.
- The lab had no external internet route by design. Attempts to check newest package updates through the guest were inconclusive.
- Two open TCP ports were observed on the intended Ubuntu target; no UDP assessment or exploit was performed.
- Cropped and redacted files are **public-release derivatives**, not unmodified forensic originals.

## Repository integrity

The private evidence archive preserves the original data and associated working checksums. Public-release copies should be re-hashed after upload if integrity verification is needed.