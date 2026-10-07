# FE-03: KYC Onboarding Wizard

**Role:** Frontend Engineer (React / Next.js)  
**Stack:** Next.js (App Router preferred), React, TypeScript, Tailwind CSS (or a comparable styling approach).

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FE-03** in your repository README and in your reply email.

## Context
Before a user can use their wallet, we must verify their identity (KYC). Users often drop off in the middle, so progress must be saved.

## Requirements

- A multi-step wizard:
  1. **Mobile number + OTP** (mock OTP `123456`, resend after 30 seconds).
  2. **Personal details:** full name, date of birth (must be 18+), email.
  3. **PAN** (format `ABCDE1234F`) and **address**.
  4. **Document upload:** ID proof image or PDF, max 2 MB, with preview.
  5. **Review & submit.**
- Validation on every step (Zod or a similar schema library is welcome). Clear inline error messages.
- Progress is saved, so a refresh or a return visit continues from the last completed step.
- Back and forward navigation between steps without losing data.
- After submit, a status page showing `PENDING_REVIEW`, `APPROVED` or `REJECTED` (with reason and a "Fix and resubmit" path).
- Mask sensitive data on the review page (for example, show PAN as `ABXXXX234F`).
- Responsive and accessible.

## Bonus
- A small admin page where a reviewer approves or rejects submissions.
- Unit tests for the validation schemas.
