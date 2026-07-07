---
name: tailor-resume
description: Tailor the user's resume and write a matching cover letter for a job description (URL, pasted text, or file path), then compile both to PDF via Typst. Reads _bank.md and preferences.md from the working directory, outputs into a per-application folder, and logs the application in applications.md. Requires a bank built by /create-resume-bank.
---

Tailor the user's resume and write a matching cover letter for this job posting: $ARGUMENTS

Work through the steps below in order. Do not skip any step.

If `_bank.md` does not exist in the working directory, stop and tell the user to run `/create-resume-bank` first.

---

## Step 0 — Load the rules

Read two files before anything else:

1. **`_bank.md`** — the single source of truth: contact info, canonical facts, profiles, summaries, skills, bullet bank, education, cover-letter material, and the Typst templates.
2. **`preferences.md`** — the user's accumulated formatting and style preferences from past feedback. Every instruction in it applies to this run and **overrides the defaults in this skill** where they conflict.

**The CANONICAL FACTS section of `_bank.md` is law.** Copy its values verbatim — header line, role titles, date strings — and never let JD language pull them off-spec. Its closed metric set is the complete list of numbers allowed to appear anywhere in the output: do not invent, round differently, combine, or extrapolate any other number, not even one the JD seems to invite.

## Step 1 — Get the job description

The argument is a URL, pasted JD text, or a path to a file in `samples/`.

- **URL**: fetch it. If it returns an error, empty content, or a login/bot-wall page, **stop immediately** and say: "Could not fetch that URL. Please paste the job description text directly into the chat." Do not retry alternate URLs; continue once the user pastes the text.
- **Text or file**: use it as-is.

From the JD, extract: company name and role title (for file naming), core responsibilities, required and preferred qualifications, JD keywords and verb phrases worth mirroring, ATS-critical terms (tool names, methodologies, certifications, role synonyms), and tone signals (technical? strategic? data-heavy? — match this in the summary).

## Step 2 — Choose a profile and plan the tailoring

**Pick one profile** from `_bank.md` `## PROFILES` based on the JD's primary emphasis, and plan:

- **Headline**: reorder the bank's headline parts to lead with what the JD emphasizes most. Substitute a part with a more specific phrase **only if** it is directly substantiated by a bullet included in this resume. Never use a headline implying a discipline the bullets don't support.
- **Summary**: start from the matching line in `## SUMMARIES`; rewrite lightly to mirror 1–2 JD-specific phrases where they fit naturally.
- **Role order**: strict reverse-chronological, always — even when an older role is more relevant. Pull the relevant content up into bullet selection, not the role itself.
- **Bullet selection**: ~3–5 bullets per recent role, tapering for older ones. Prefer bullets sharing vocabulary with the JD. Use the bullet variant matching the JD's flavor where the bank provides variants. Don't starve the user's longest or most substantial role to make room for a recent one.
- **Bullet rewrites**: where a bullet is close but not quite right, rewrite it to lead with a JD-relevant term or verb — but never invent metrics or claim ownership beyond what the bank supports.
- **Bullet style — plain, result-first**: each bullet reads as action then outcome, in working prose, not marketing copy. Lead with the result when there's a metric. Cut marketing nouns and intensifiers ("growth engine", "unlocked", "world-class", "supercharged", "10x"). No arrows (`→`, `->`) or slash-shorthands ("BE/FE", "signup→purchase") as connectors — use words. Minimize em dashes. The goal is prose that reads cleanly aloud.
- **Skills section**: reorder skill categories per the chosen profile. Echo JD tool/methodology names within existing skill lines where accurate.

**Keyword honesty.** Track which JD-critical keywords appear naturally in your selected content. Every still-missing keyword has exactly three fates:

1. **Weave it into a bullet or summary** if the user's actual work supports it (preferred).
2. **Add it to a SKILLS line** if it's a tool/method the user has genuinely touched.
3. **Leave it out and flag it as a gap** in the tailoring summary — an honest gap noted for interview prep beats a keyword the user can't defend in a screen.

There is no fourth option. **Hidden text is banned**: no white/1pt/off-page keyword blocks, no `#hide`. Modern ATS flag hidden text, and the parsed plain-text a recruiter sees renders it as a wall of buzzwords.

**Start every tailoring from the bank, never from a previously generated resume** — prior outputs carry another JD's vocabulary. And never write this JD's phrases back into `_bank.md`; the bank stays JD-neutral.

## Step 3 — Derive the output folder

Slug: `<company>-<role-keyword(s)>` — lowercase, hyphens, max ~5 words (e.g., `stripe-growth-pm`). Create `<slug>/` in the working directory. Outputs:

- `<slug>/resume_<slug>.typ` and `<slug>/cover_letter_<slug>.typ`
- `<slug>/<First>_<Last>_<Company>_CV.pdf` and `..._CL.pdf` — names from CANONICAL FACTS; `<Company>` with spaces as underscores, in the company's own casing. PDFs are named after the person, not the slug, so they're recognizable to a recruiter at a glance.

## Step 4 — Write the tailored resume

Write `<slug>/resume_<slug>.typ` as a complete, standalone Typst file.

- Copy the `#set`/`#show` rules and the `role()` / `school()` helpers **verbatim from `_bank.md` `## TYPST_RESUME_TEMPLATE`**. The template carries deliberately tuned spacing — do not re-derive or "improve" it (keep the negative `v(-0.75em)` in the level-2 heading rule; it makes the rule line hug the section title).
- Add `#v(0.22em)` on its own line immediately before `== EXPERIENCE` and `== EDUCATION` (only those two).
- **Header**: name as a level-1 heading, then the contact line and headline each wrapped in `#align(center)[...]` — without the wrapper they render left-aligned. Put `#v(0.45em)` between the contact line, the headline, and the summary paragraph — tighter than that reads crowded. The summary paragraph stays unwrapped (left-aligned).
- **Skills lines**: the global `#set par(spacing: 0pt)` kills paragraph gaps — put `#v(0.5em)` between skill lines (not after the last).
- **Escapes**: write `\~` for a literal `~` (bare `~` is a non-breaking space in Typst) and `\@` inside email addresses.
- **Structure**: name + contact → headline → summary → `== EXPERIENCE` → `== SKILLS` → `== EDUCATION` (unless `preferences.md` reorders it).
- **Page length**: default to one page. If it runs over, trim the weakest recent-role bullets and tighten wording — never shrink spacing values to force a fit. A second page is allowed only when genuinely relevant content earns it, and never less than about a third full.

## Step 5 — Write the cover letter

Write `<slug>/cover_letter_<slug>.typ` as a complete, standalone Typst file, copying the page settings and header block verbatim from `_bank.md` `## TYPST_COVER_LETTER_TEMPLATE`.

Four paragraphs, one page:

- **P1 — Hook + motivation + fit**: name the specific role; state the user's genuine motivation for this company (from `COVER_LETTER_EXTRAS`) tied to 1–2 specifics from the JD; close with the clearest one-sentence overlap between their background and the role.
- **P2 — Strongest proof point**: the most relevant accomplishment, framed around how the user thinks and what they owned, with numbers only from the closed metric set. Lead with reasoning and outcome, not process mechanics.
- **P3 — Second proof point**: a second matching area, plus any relevant background thread from the bank (career change story, domain background) if the JD invites it.
- **P4 — Close**: genuine interest in the specific problem this role solves, echoing P1's motivation. One or two sentences, inviting a conversation.

Rules: reference 2–3 specific phrases or priorities from the JD; plain language with the same style rules as bullets (no buzzword stacks, no arrows, minimal em dashes; spell out approximations — "about 70%", not "~70%"); date it today; salutation `Dear [Company Name] Hiring Team,` unless a name is known; never claim ownership or motivation the bank doesn't support. **Forward-looking aspirations are honest ("I want to move deeper into X") — claims of past work the user hasn't done are not.** Keep that line bright.

## Step 6 — Pre-flight checklist

Verify against the `.typ` files you just wrote, before compiling:

- [ ] Header line matches CANONICAL FACTS verbatim, on both documents.
- [ ] Every role title and date string matches CANONICAL FACTS exactly; "Present" appears exactly once (search the file).
- [ ] Roles are in reverse-chronological order.
- [ ] Every number traces to the closed metric set — search for digits and `%` and confirm none were invented, re-rounded, or combined.
- [ ] No inflated title anywhere — search for "Director", "Head", "VP", "Principal", "Staff", "Lead" and confirm any hit is a title the user actually held.
- [ ] No hidden text — search for `fill: white`, `1pt`, `#hide`, off-page runs; zero hits.
- [ ] No leftover vocabulary from a different JD; every prominent term traces to this JD and a real bank accomplishment.
- [ ] Bullet style holds: result-first plain prose, no marketing intensifiers, no arrows or connector-slashes.
- [ ] Every instruction in `preferences.md` has been applied.
- [ ] PDF filenames follow `<First>_<Last>_<Company>_CV.pdf` / `_CL.pdf`.

## Step 7 — Compile to PDF

```
typst compile "<slug>/resume_<slug>.typ" "<slug>/<First>_<Last>_<Company>_CV.pdf"
typst compile "<slug>/cover_letter_<slug>.typ" "<slug>/<First>_<Last>_<Company>_CL.pdf"
```

- If compilation fails, print the Typst error, fix the source `.typ`, and re-run — never leave a broken PDF.
- If `typst` is not installed, say so, point the user to the install line in the README (`winget install --id Typst.Typst` / `brew install typst`), and offer an interim `resume_<slug>.md` markdown version they can paste into Google Docs. The `.typ` files still get written either way.

## Step 8 — Log the application

Append one row to `applications.md` in the working directory (create it from the table header in that file's template if missing):

`| YYYY-MM-DD | Company | Role title | <slug>/ | JD link or "pasted" | applied |`

Don't overwrite existing rows. If a row for the same company+role already exists, update it instead of duplicating.

## Step 9 — Print a tailoring summary, then ask for feedback

```
Profile: [name] — [one sentence why it fits this JD]
Headline: [the reordered headline used]
Top JD keywords incorporated: [5–7 terms and where they appear]
Keyword gaps (for interview prep): [JD-critical terms honestly left out, or "none"]
Bullets flagged for interview prep: [any [confirm ownership] bullets used, or "none"]
Output folder: <slug>/
```

Then ask the user to open the PDF and share anything they'd change — length, ordering, tone, formatting, wording. **When they give feedback that should apply to every future resume, append it to `preferences.md`** as a short imperative rule with a one-line "why" (e.g., "Always show 'San Francisco, CA' in the header, not my suburb — recruiters filter by metro"). One-off changes for this application only get applied to the current `.typ` files without touching `preferences.md`. Never edit this skill file or `_bank.md` to record preferences.

## Guardrails

- Never invent metrics, tools, titles, or experiences absent from `_bank.md`. The metric set is closed.
- Never inflate seniority; use only exact titles from CANONICAL FACTS.
- Never use hidden text, in any form.
- Prefer achieved outcomes over targeted ones; a bullet with only a target gets flagged, not dressed up as a result.
- Never claim sole ownership of `[confirm ownership]` or team-attributed work — keep language inclusive ("led", "partnered", "my team shipped").
- 4 strong bullets beat 6 weak ones; do not pad.
- Plain language wins: no jargon the JD didn't ask for, and every trendy term must trace to this JD AND a real bank accomplishment.
- If the JD asks for something clearly outside the user's background, report it as a gap in the summary — never paper over it.
