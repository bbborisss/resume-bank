# resume-bank

Tailor your resume and cover letter to any job description in one command, without ever
letting an AI invent your work history.

This is a template repository for [Claude Code](https://claude.com/claude-code). You build
a **resume bank** once — every title, date, bullet, and metric you can actually defend in
an interview — and then each application is one command: paste a job posting, get back a
tailored one-page resume and cover letter as polished PDFs, plus an honest list of the
keywords that were left out because your background doesn't support them.

## Why this works differently

Most AI resume tools optimize for keyword density and quietly drift into fiction. This one
is built around an **anti-hallucination lock**: your bank contains a canonical-facts section
with your exact titles, exact dates, and a *closed set* of metrics. The tailoring skill may
reorder, reword, and emphasize — it may never invent a number, inflate a title, or hide
keywords in white text. Anything the job description asks for that you don't have gets
flagged as a gap for interview prep instead of papered over.

## Requirements

- [Claude Code](https://claude.com/claude-code) (CLI, desktop app, or claude.ai/code)
- [Typst](https://typst.app) for PDF compilation — one line to install:
  - Windows: `winget install --id Typst.Typst`
  - macOS: `brew install typst`
  - Linux: see [typst releases](https://github.com/typst/typst/releases)

  (No Typst? The skill still writes the source files and can give you a markdown version
  to paste into Google Docs.)

## Quickstart

1. **Click "Use this template"** on GitHub to create your own copy — **make it private**;
   it will contain your phone number, work history, and application log.
2. Clone your copy and open Claude Code in it.
3. Drop your current resume (any format) and any supporting material — old resumes,
   LinkedIn export, brag docs, performance reviews — into a `source-materials/` folder.
4. Run **`/create-resume-bank`**. Claude parses everything, then interviews you: exact
   titles and dates, more bullets per role than your resume ever had room for, and a
   "could you defend this number in an interview?" check on every metric. The result is
   your `_bank.md`.
5. **Calibrate**: run `/tailor-resume samples/sample-jd-<closest-to-your-target>.md`,
   open the PDF, and tell Claude everything you'd change. Your feedback is saved to
   `preferences.md` and applied to every future run.
6. Apply for real: **`/tailor-resume <job posting URL or pasted text>`**. Each run creates
   a per-application folder with the `.typ` sources and recruiter-ready PDFs, and logs a
   row in `applications.md`.

## What's in the box

```
.claude/skills/
  create-resume-bank/   # one-time setup interview → builds _bank.md
  tailor-resume/        # per-application tailoring → PDFs + tracker row
templates/              # bank skeleton, preferences file, proven Typst layout
samples/                # three fictional JDs (PM, engineer, marketer) for calibration
applications.md         # lightweight application tracker (a markdown table)
```

Files created as you use it (yours, not the template's): `_bank.md`, `preferences.md`,
`source-materials/`, and one folder per application.

## The three layers

| Layer | File(s) | Who writes it |
|-------|---------|---------------|
| Skills — the generic process | `.claude/skills/` | this repo (updateable) |
| Your facts — the single source of truth | `_bank.md` | `/create-resume-bank` + you |
| Your taste — accumulated feedback | `preferences.md` | `/tailor-resume` as you give feedback |

Skills never contain your data, and tailoring never edits the skills or the bank — so you
can pull upstream skill improvements into your copy without touching your history:

```
git remote add upstream https://github.com/bbborisss/resume-bank.git
git fetch upstream && git merge upstream/main
```

## The tracker

`applications.md` is a plain markdown table — one row per application, appended
automatically. Update it conversationally ("mark Northwind as interviewing") or by hand.
If you already track applications elsewhere, ignore or delete it.

## Honesty guarantees (read these — they're the point)

- **Closed metric set**: no number appears on any output that isn't in your bank, confirmed by you as defensible.
- **No title inflation**: exact titles only, the ones HR would verify.
- **No hidden text**: white-text/1pt keyword stuffing is banned outright. Modern ATS flag it, and recruiters see it in the parsed plain text.
- **Gaps are reported, not papered over**: every run ends with a list of JD keywords that were honestly left out — that list is your interview-prep homework.
- **Ownership stays honest**: shared work is flagged in the bank and always described inclusively ("led", "partnered", "my team shipped").

## Privacy

Your copy of this repo is your job-hunt database. Keep it **private**. Nothing here sends
your data anywhere beyond your normal Claude Code usage, but a public fork would publish
your phone number and complete work history. If you must keep your repo public for some
reason, add `_bank.md`, `preferences.md`, `source-materials/`, `applications.md`, and your
application folders to `.gitignore` (a commented-out block is ready in the file).

## Contributing

Issues and PRs welcome — especially generic skill improvements, additional sample JDs, and
alternative Typst templates. Please don't submit anything containing real personal data.

## License

[MIT](LICENSE)
