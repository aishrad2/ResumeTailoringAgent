# Requirements: Resume Tailoring Agent

## What it does

Takes your resume and a job posting and suggests rewritten bullets that match
the posting. You review every change in a local web page and accept or reject
each one. Then it makes a PDF from the approved text only.

It can reword and reorder what's already there, but it **never adds anything
new**. Everything runs on your own computer, and nothing is stored.

## How it works

```
resume.yaml + job posting → clean inputs → match bullets → local AI rewrites
→ automatic checks → YOU review each change in the browser → PDF
```

## Assumptions

- Your computer is trusted. Malware on your own machine is out of scope.
- The only files involved are `resume.yaml` going in and the PDF coming out.

## Rules

| # | Rule | How I'll check it |
|---|---|---|
| 1 | The resume comes from `resume.yaml`, loaded safely and checked for errors (missing fields, duplicate IDs, files that are too big). | Tests with bad files |
| 2 | The job posting is cleaned (hidden characters and HTML removed) and treated as untrusted text, never as instructions. | Test with a posting that says "ignore your instructions" |
| 3 | The AI runs locally, has no tools, and has no internet access. | Test with network blocked |
| 4 | The AI must reply in a fixed JSON format. Anything else is rejected. | Test with a fake AI that returns junk |
| 5 | Automatic check: each rewrite points to a real original bullet and adds no new numbers, skills, employers, titles, dates, links, emails, or phone numbers. | Tests with a fake AI that tries to cheat |
| 6 | If a rewrite fails that check, the original bullet is used and flagged. | Same tests |
| 7 | The review page shows each change as original vs. tailored, with changed words highlighted. Risky changes get a badge (new numbers, tools, or ownership words like "led"). Code computes the highlights, not the AI. | Tests on sample before/after pairs |
| 8 | You must accept or reject every change. The PDF button stays disabled until you do, and there is no "accept all." Reject means the original bullet is used. | UI test |
| 9 | The PDF is built only from approved text, exactly as shown. Text is escaped, nothing external is loaded, and there is no hidden text. | Test with hostile text, then inspect the PDF |
| 10 | The review page runs only on `127.0.0.1`, requires a random token, and rejects unexpected `Host` headers. | Tests for each case |
| 11 | Nothing is remembered: no database, history, or content in logs, and no temp files left behind. | Check folders and logs after a run |
| 12 | If anything fails, no PDF is made. | Tests with injected failures |
| 13 | The repo has only a fake sample resume. Real data is gitignored. gitleaks, bandit, and pip-audit run on every push. | GitHub Actions |

## Setup (one time)

- Pin exact versions of all Python packages.
- Download only from official sources: PyPI, the Ollama library, Hugging Face.
- Write down the exact model names in the README.

## Done when

- A sample resume and job posting go through review and produce a PDF.
- A deliberately cheating fake AI is blocked.
- A prompt-injection posting does not change the output.
- The PDF can't be made until every change has been reviewed.

## Not included (for now)

- PDF resume input, scanned documents, OCR
- Editing text during review (accept or reject only)
- Any remote AI service, accounts, or saved history

## Open questions

- How to spot a new skill or tool in a rewrite (needs a list of known skills).
- Size limits (suggested: resume file 100 KB, job posting 20,000 characters).
- How many bullets to keep per section.