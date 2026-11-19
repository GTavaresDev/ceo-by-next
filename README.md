# Agent: SEO for Next.js (NEST course)

## Purpose

This agent is a knowledge base built from the NEST SEO course for Next.js developers. It answers questions about how search engines discover, index, render, and rank a web app, and how to apply those practices in a Next.js project.

## Structure

```
agent-ceo-nest/
├── README.md                      # Purpose, structure, status
├── eixos/                         # Learnings organized by axis
│   ├── 01-foundations.md          # Why SEO matters, the three pillars (Ch. 1)
│   ├── 02-search-systems.md       # Search systems, Googlebot pipeline (Ch. 2-3)
│   ├── 03-crawling-indexing.md    # Status codes, robots.txt, sitemaps, meta tags, canonical (Ch. 4-9)
│   ├── 04-rendering-ranking.md    # Rendering strategies, AMP (Ch. 10-12)
│   ├── 05-creation.md             # URLs, metadata, headings (Ch. 13-15)
│   ├── 06-popularity.md           # Backlinks, internal links, PageRank (Ch. 15)
│   ├── 07-web-performance.md      # Core Web Vitals, page experience, SEO impact (Ch. 16-22)
│   ├── 08-optimization-techniques.md  # next/image, dynamic imports, fonts, third-party scripts (Ch. 24-28)
│   └── 09-measurement-monitoring.md   # Lighthouse, Speed Insights, reportWebVitals, CrUX (Ch. 23, 29-33)
└── fontes.md                      # Source list and references
```

## Axes

| Axis | Pillar | Status |
|------|--------|--------|
| Foundations | General | Covered (Ch. 1) |
| Search systems and Googlebot | Technical | Covered (Ch. 2-3) |
| Crawling and indexing | Technical | Covered (Ch. 4-9) |
| Rendering and ranking | Technical | Covered (Ch. 10-12) |
| Creation (URLs, metadata, headings) | Creation | Covered (Ch. 13-15) |
| Popularity (links) | Popularity | Covered (Ch. 15). Outreach process pending |
| Web performance | Technical | Covered (Ch. 16-22) |
| Optimization techniques | Technical | Covered (Ch. 24-28) |
| Measurement and monitoring | Technical | Covered (Ch. 23, 29-33) |

## Conventions

- All agent content is written in English.
- Each axis file has **Key takeaways** and a **Next.js checklist**.
- Examples follow the course's code, mostly Pages Router (`getStaticProps`, `next/head`, `next/link` with `<a>`). Verify against current Next.js docs before applying them to App Router projects.
- Facts about Google's behavior come from the course and Google's documentation. Some are out of date (for example, FID was replaced by INP). Check the links in `fontes.md` for anything time-sensitive.

## Known gaps

- Creation: the course's outreach and link-building lessons are not provided yet.
- Course content is complete through Chapter 33 (the final chapter).

## Changelog

- v0.4: Added Ch. 21-33. Page experience, optimization techniques, measurement and monitoring. Created `08-optimization-techniques.md` and `09-measurement-monitoring.md`.
- v0.3: Added Ch. 10-20. Rendering, URLs, metadata, headings, internal links, Core Web Vitals. Created `07-web-performance.md`.
- v0.2: Ch. 1-9 and the Googlebot diagram.
- v0.1: Initial structure.
