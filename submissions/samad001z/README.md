# samad001z · Recovery Manager

Copy this file to `submissions/samad001z/README.md` and keep it as your index.

## Expected layout

```
submissions/samad001z/
├── README.md            ← this file: who you are, links to everything below
├── 01-customer-letter.md
├── 02-prfaq.md          ← include the questions you'd rather not answer
├── 03-one-pager.md      ← metrics table + at least one kill condition
├── CLAUDE.md            ← durable constraints, hard rules, forbidden language
├── build-brief.md
├── build-log.md         ← keep it current; organisers read it
├── eval-report.md       ← method, two-labeller agreement, per-check FP / FN, failure modes
├── contract/            ← your evidence-record shape, as agreed with the other pods
└── agent/               ← your code (headless first)
```

## Status

| Face | Deliverable | Status |
|---|---|---|
| 1 | Customer letter, PR/FAQ, one-pager | ☑ first draft |
| 2 | CLAUDE.md | ☐ |
| 3 | Headless agent on fixtures | ☐ |
| 4 | Eval report | ☐ |
| 5 | Evidence record page | ☐ |
| 6 | Cross-pod contract | ☐ |

## Kill condition

K1: if claim precision on 50 held-out charges is below 90% after two fix rounds, stop drafting claims. (K2 and K3 in 03-one-pager.md.)
