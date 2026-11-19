# SEO audit for Next.js

This repository is a standard for reviewing SEO on a Next.js app. The notes come from the [Next.js SEO course](https://nextjs.org/learn/seo/next-steps): how a search engine discovers, indexes, renders, and ranks a site, and which practices to apply in Next.js, including Core Web Vitals and measurement.

The course notes live in `.agent/`. They stay on disk during normal work. A skill named `nextjs-seo-audit` reads them only when you ask for a final review. The same procedure exists for Cursor, Claude, and Antigravity. The skill reports what to fix and leaves the code unchanged unless you ask it to apply the fixes in the same message.

## How to use the skills

Open the Next.js app you want reviewed in a workspace that also includes this repository, so the skill can read `.agent/`. Run the audit after the implementation is done.

- **Cursor:** `Use the nextjs-seo-audit skill and analyze what needs to be fixed in this project.` The skill file is `.cursor/skills/nextjs-seo-audit/SKILL.md`.
- **Claude:** `/nextjs-seo-audit`. The skill file is `.claude/skills/nextjs-seo-audit/SKILL.md`.
- **Antigravity:** `/nextjs-seo-audit`. The skill file is `.agents/skills/nextjs-seo-audit/SKILL.md`. If the command is missing from a CLI session that was already open, run `/skills reload`.

Leave `.agent` out of an ordinary prompt. Attaching that folder pulls the whole course into context. The skill answers in the language of your message.

## What the audit checks

Each file in `.agent/eixos/` has key takeaways and a Next.js checklist. The course samples are mostly Pages Router. The skill translates them before judging an App Router app.

| Axis | File | Course |
|------|------|--------|
| Foundations | `01-foundations.md` | Ch. 1 |
| Search systems and Googlebot | `02-search-systems.md` | Ch. 2–3 |
| Crawling and indexing | `03-crawling-indexing.md` | Ch. 4–9 |
| Rendering and ranking | `04-rendering-ranking.md` | Ch. 10–12 |
| Creation (URLs, metadata, headings) | `05-creation.md` | Ch. 13–15 |
| Popularity (links) | `06-popularity.md` | Ch. 15 |
| Web performance | `07-web-performance.md` | Ch. 16–22 |
| Optimization techniques | `08-optimization-techniques.md` | Ch. 24–28 |
| Measurement and monitoring | `09-measurement-monitoring.md` | Ch. 23, 29–33 |

Sources and corrections, including the replacement of FID by INP, are in `.agent/fontes.md`. Outreach and keyword research are outside this standard. Those lessons are not in the course notes yet.

## Layout

```
├── README.md
├── .agent/                          # Course notes, read only during an audit
│   ├── fontes.md
│   └── eixos/
├── .cursor/skills/nextjs-seo-audit/SKILL.md
├── .claude/skills/nextjs-seo-audit/SKILL.md
└── .agents/skills/nextjs-seo-audit/SKILL.md
```
