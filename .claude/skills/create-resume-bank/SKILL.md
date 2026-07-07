---
name: create-resume-bank
description: Build a personal resume bank (_bank.md) from the user's existing resume and supporting materials. Parses everything they provide, interviews them to fill gaps and lock down facts, then generates _bank.md and preferences.md and walks them into a calibration run of /tailor-resume. Run this once when setting up the repo, or again to do a full rebuild.
---

Build (or rebuild) the user's resume bank. Arguments, if any, point at source materials: $ARGUMENTS

You are building the **single source of truth** that every future tailored resume will be generated from. Errors baked in here get repeated on every application, so this process favors asking over assuming. Work through the steps in order.

---

## Step 1 — Collect source materials

Gather everything the user can give you:

- Files passed as arguments or attached to the conversation
- Anything in a `source-materials/` folder in the working directory (tell the user they can drop files there: current resume in any format, old resumes, LinkedIn profile export or pasted text, brag documents, performance reviews, portfolio links, old cover letters)
- If the user gave you nothing at all, ask for at minimum their **current resume** (pasted text is fine) before continuing. Do not build a bank from an empty interview — parsing real material first produces far better questions.

Read every file provided. For PDFs and images, read them directly; for URLs (e.g., a portfolio), fetch them.

## Step 2 — Parse into a draft structure

From the materials, extract a draft of:

- **Contact info**: name, city/state, email, phone, LinkedIn, portfolio/GitHub
- **Roles**: title, company, location, dates, and every bullet/accomplishment mentioned anywhere (dedupe across documents, but keep distinct variants of the same accomplishment — they become bullet variants later)
- **Metrics**: every number attached to an accomplishment, with where it came from
- **Skills**: tools, methods, languages, certifications
- **Education**: schools, degrees, dates, honors
- **Narrative material**: anything useful for cover letters — career motivations, background stories, notable non-work experience

Present this draft back to the user as a compact summary so they can see what you found before the interview starts.

## Step 3 — The interview

This is the heart of the setup. Ask in **batches of 3–5 questions**, wait for answers, then continue. Do not fire 30 questions at once. Cover all of the following areas:

**A. Contact and header**
- Confirm the exact contact line (city/state to show, email, phone, links). Note: the city they show is a choice — remote-friendly candidates often show a metro area rather than a suburb. Whatever they pick becomes canonical.

**B. Per role (walk newest to oldest)**
- Is this your most current position? What's your exact, official title — the one HR would verify? (Explain why this matters: an inflated title that fails a background or reference check reads as misrepresentation.)
- Exact start and end months. Only ONE role may end in "Present".
- "Add as many bullets as possible for this role" — push for more than what's on the resume: projects, launches, fires put out, things they're proud of that never made the page. The bank should hold 2–3× more bullets than any single resume will use.
- For each metric: **"Could you defend this number in an interview?"** If yes, it goes in the closed metric set. If it's a guess, either soften it to defensible language or drop it.
- Ownership check: for each major accomplishment — did you lead it, partner on it, or was it a team effort you contributed to? Flag anything ambiguous with `[confirm ownership]` so tailoring never claims sole credit for squad work.

**C. Skills and gaps**
- "What other skills do you have that aren't on the resume?" — tools, methods, domain knowledge, languages. Apply the bar: only skills they've **demonstrably used** go in; aspirational skills stay out.

**D. Targeting**
- What roles/titles are you applying for? (This defines the 2–4 **profiles** — e.g., "growth", "platform", "management" — each with its own summary emphasis and skill ordering.)
- Any constraints: location/remote, seniority range, industries to avoid.

**E. Cover letter material**
- Why are you job hunting / what do you want next? (Their honest motivation thread — this makes cover letters non-generic.)
- 1–2 background stories worth having on tap (career change, unusual path, relevant side work).

## Step 4 — Generate the bank

Write `_bank.md` in the working directory, using `templates/_bank.template.md` as the structural skeleton. Critical sections:

- **CANONICAL FACTS** — the anti-hallucination lock. Generated from the interview answers, it fixes forever:
  - The exact contact/header line, verbatim
  - Every role title, exactly as confirmed (with a note: "never inflate — no Director/Head/VP/Lead unless actually held")
  - Every role's exact date string, and which single role ends in "Present"
  - **The closed metric set**: an explicit list of every defensible number. The rule written into the file: *no number may ever appear on a resume that is not in this list.*
- **PROFILES / SUMMARIES / SKILLS / EXPERIENCE_ROLES / EDUCATION / BULLETS / COVER_LETTER_EXTRAS** — filled from the parse + interview. Bullets keyed with short IDs, ownership flags preserved, variants grouped.
- **Typst templates** — copy verbatim from the template file; do not modify formatting values.

Also copy `templates/preferences.template.md` to `preferences.md` if it doesn't already exist.

Then show the user the CANONICAL FACTS section and ask them to confirm every line. Fix anything they correct before finishing.

## Step 5 — Kick off calibration

Tell the user, in roughly these words:

> Your bank is ready, but the tailoring isn't calibrated to your taste yet. Pick the sample job description in `samples/` closest to your target role (or use a real posting) and run:
>
> `/tailor-resume samples/<file>`
>
> Then open the PDF and tell me everything you'd change — length, section order, tone, formatting, wording style. I'll save your feedback to `preferences.md`, and every future run will apply it automatically.

## Guardrails

- Never invent anything to fill a gap — a thin, honest bank beats a padded one. Empty sections are fine.
- Never put a number in the closed metric set that the user didn't confirm as defensible.
- Never write the user's materials or bank contents anywhere outside the working directory.
- If the user runs this command when `_bank.md` already exists, ask whether they want a full rebuild or targeted updates (new role, new bullets) — for targeted updates, edit the existing bank instead of regenerating it.
