# FE-03: KYC Onboarding Wizard

**Role:** Frontend Engineer (React / Next.js) · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **FE-03** in your repository README and in your reply email.

## Scenario

Before using their wallet, a user must verify their identity (KYC). Many users stop halfway, so progress must be saved.

## Must have

- Next.js + TypeScript.
- A 4-step wizard:
  1. **Mobile + OTP** (mock OTP `123456`)
  2. **Personal details:** full name, date of birth (must be 18+), email
  3. **PAN** (format `ABCDE1234F`)
  4. **Review & submit**, with the PAN masked (for example `ABXXXX234F`)
- Validation on every step with clear inline errors (Zod or a similar library is welcome).
- Back and Next without losing data.
- Progress is saved, so a refresh continues from the same step.
- A final status screen after submit.

## Bonus (optional)

- Document upload step (image or PDF, max 2 MB, with preview).
- Resend OTP with a 30-second timer.
- Unit tests for the validation rules.

## Answer in your README (a few lines each)

1. Where did you save progress, and what are the security risks of storing personal data there?
2. How would you make this wizard accessible to screen-reader users?

If you run out of time, list what you skipped and how you would build it.
