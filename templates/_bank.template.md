# RESUME BANK — {Full Name}

This file is the single source of truth for every tailored resume and cover letter.
It is generated and maintained by `/create-resume-bank` — edit it deliberately, never
let a single job description's vocabulary leak into it.

## CANONICAL FACTS (anti-hallucination lock)

These values are fixed across every resume and cover letter. Copy them verbatim; never
infer, "improve", or let JD language pull them off-spec.

- **Header line (exact, both documents):** `{City, ST} | {email} | {phone} | {linkedin.com/in/...}`
- **Role titles (exact — never inflate):**
  - `{Exact Title}` at {Company} — has NEVER held: {e.g., Director, Head, VP, Principal, Staff, Lead}
  - `{Exact Title}` at {Company}
- **Role dates (exact strings; exactly ONE role ends in "Present"):**
  - `{Exact Title} | {Company}` → `{Mon YYYY} - Present`
  - `{Exact Title} | {Company}` → `{Mon YYYY} - {Mon YYYY}`
- **The metric set is closed.** The only numbers that may appear anywhere in a resume or
  cover letter are the ones listed here, each confirmed defensible in an interview:
  - {e.g., "grew signups 32% in two quarters"}
  - {e.g., "$1.2M annual cost reduction"}
  - {…every defensible number, one per line}

  Do NOT invent, round differently, combine, or extrapolate any other number — not even
  one the JD seems to invite.

## HEADLINE_PARTS (reorder to match JD emphasis)

{Part 1} | {Part 2} | {Part 3}

Each part must be substantiated by at least one bullet on any resume that uses it.

## PROFILES

- **{profile-name}** (e.g., growth): for JDs emphasizing {…}. Summary: `{summary-id}`. Skill order: {Category A, Category B, Category C}.
- **{profile-name}**: …
- **{profile-name}**: …

## SUMMARIES

- `{summary-id}`: {2–3 sentence summary paragraph tuned to this profile.}
- `{summary-id}`: {…}

## SKILLS

- *{Category A}:* {tools and methods, comma-separated — only things demonstrably used}
- *{Category B}:* {…}
- *{Category C}:* {…}

## EXPERIENCE_ROLES

- `{role-id}`: {Exact Title} | {Company} | {Location} | {dates} — bullets: [{bullet-ids}]
- `{role-id}`: {…}

## EDUCATION

- {School} | {Location} | {Degree}{, honors} | {dates}

## COVER_LETTER_EXTRAS (background facts for cover letters, not on the resume)

- **Motivation thread:** {what the user honestly wants next, in their words — the spine of P1}
- **Background stories:** {1–2 stories worth having on tap: career change, unusual path, relevant side work}
- {Other facts safe to use in letter bodies}

## BULLETS

Format: `[bullet-id]` (role, ownership flag if any) followed by the bullet text and variants.

- `[{bullet-id}]` ({role-id}): {Result-first bullet text: action then outcome, metric from the closed set.}
  - variant ({flavor, e.g., technical}): {same accomplishment, reworded for a different JD flavor}
- `[{bullet-id}]` ({role-id}) `[confirm ownership]`: {Bullet where credit is shared — tailoring must use inclusive language.}

## JD_NOTES

Durable, JD-neutral framing notes that help tailoring (e.g., "the {X} work maps well onto
platform JDs"). Never paste a specific JD's phrases here.

## TYPST_RESUME_TEMPLATE
```typst
#set page(paper: "us-letter", margin: (x: 0.55in, y: 0.5in))
#set text(size: 9.8pt)
#set par(leading: 0.52em, spacing: 0pt)
#set list(indent: 0.15in, body-indent: 0.12in, spacing: 0.52em)
#set heading(numbering: none)

#show heading.where(level: 1): it => align(center)[
  #text(size: 18pt, weight: "bold", tracking: 0.5pt)[#it.body]
  #v(0.5em)
]

#show heading.where(level: 2): it => {
  v(0.9em)
  text(size: 10.5pt, weight: "bold", tracking: 0.35pt)[#it.body]
  v(-0.75em)
  line(length: 100%, stroke: 0.45pt)
  v(0.48em)
}

#let role(title, company, location, dates, body) = [
  #table(
    columns: (1fr, auto),
    inset: 0pt,
    stroke: none,
    align: (left, right),
    [*#title* | #company | #location],
    [#dates],
  )
  #v(0.55em)
  #body
  #v(0.5em)
]

#let school(name, location, degree, dates) = [
  #table(
    columns: (1fr, auto),
    inset: 0pt,
    stroke: none,
    align: (left, right),
    [*#name* | #location],
    [#dates],
  )
  #v(0.22em)
  #degree
  #v(0.5em)
]
```

## TYPST_COVER_LETTER_TEMPLATE
```typst
#set page(paper: "us-letter", margin: (x: 0.75in, y: 0.7in))
#set text(size: 10.5pt)
#set par(leading: 0.55em, spacing: 0.9em, justify: false)

#align(center)[
  #text(size: 16pt, weight: "bold", tracking: 0.4pt)[{FULL NAME}]

  {City, ST} | {email with \@ escaped} | {phone} | {linkedin.com/in/...}
]

#v(1.2em)
```
