---
name: nextjs-seo-audit
description: >-
  Final SEO audit of a Next.js project against the NEST course (crawling,
  indexing, metadata, Core Web Vitals, measurement). Use only when the user
  types /nextjs-seo-audit or explicitly names nextjs-seo-audit for a final
  auditoria SEO. Do not use it while implementing features.
---

# Next.js SEO audit

On-demand review. The user runs this with `/nextjs-seo-audit` after the work is done. Do not read the course files on an ordinary coding turn.

The course notes are the standard. This file is only the audit procedure. Do not edit project code. Deliver the report. Apply fixes only when the same message asks for them.

Reply in the language of the user's message.

## Knowledge base

Read these files in this repo before judging the project. If `.agent/` is missing, stop and say so.

- `.agent/fontes.md` — read **Corrections to the course** and **Gaps** first
- `.agent/eixos/01-foundations.md`
- `.agent/eixos/02-search-systems.md`
- `.agent/eixos/03-crawling-indexing.md`
- `.agent/eixos/04-rendering-ranking.md`
- `.agent/eixos/05-creation.md`
- `.agent/eixos/06-popularity.md`
- `.agent/eixos/07-web-performance.md`
- `.agent/eixos/08-optimization-techniques.md`
- `.agent/eixos/09-measurement-monitoring.md`

Use each axis **Next.js checklist** as the pass/fail list. Use **Key takeaways** to explain a failure. Ignore keyword research and link outreach. The course marks those lessons as not provided.

## Course facts that are stale

Apply `fontes.md` over the axis examples:

- Interactivity vital to audit is **INP**, not FID. FID is historical. In lab data, use TBT as the proxy.
- Lighthouse weights in axis 07 are Lighthouse v6. Do not treat them as current.
- `next/link` no longer needs an `<a>` child or `passHref`.
- Fonts: `next/font`, not the older font pattern in the course.
- AMP was removed. Do not recommend it.
- `reportWebVitals` is Pages Router. App Router uses `useReportWebVitals`.
- Google Data Studio is now Looker Studio.

## Detect the router before applying a checklist

Course samples are mostly Pages Router (`next/head`, `getStaticProps`, `pages/sitemap.xml.js`).

| Signal | Router |
| --- | --- |
| `app/` with `layout.tsx` or `page.tsx` | App Router |
| `pages/` with `next/head` | Pages Router |
| Both | Audit each tree with its own API. Do not fail App Router code for missing `next/head`. |

Translate a failing Pages Router check into the current API, then look for that:

- Title, description, Open Graph, canonical, robots: `metadata` or `generateMetadata` in `layout.tsx` / `page.tsx`. `next/head` only on Pages Router. Metadata must be unique per indexable URL.
- `robots.txt`: `app/robots.ts` or `public/robots.txt`.
- Sitemap: `app/sitemap.ts` or `public/sitemap.xml` (Pages: `pages/sitemap.xml.js`).
- One `h1` per page, aligned with the title.
- Redirects and status: `redirects` / `headers` in config, `redirect()`, `notFound()`, route handlers. Indexable pages should end at 200. Permanent moves use 301/308. `robots.txt` does not de-index; `noindex` does.
- Images: `next/image` with explicit dimensions, not raw `<img>`.
- Fonts: `next/font`. Third-party scripts: `next/script` with `afterInteractive` or `lazyOnload`.
- Heavy client code: `next/dynamic` or `import()` when it is not needed on first paint.
- Internal links: `next/link` on in-app URLs.
- Measurement: Lighthouse on a production build, plus a field path (Speed Insights, CrUX, or `useReportWebVitals` / `reportWebVitals`).

Confirm rendering from the route (static, dynamic, `generateStaticParams`, server vs client components). Client-only content that should be indexed is a finding. Do not recommend AMP.

## What to inspect

Read the project. Do not score from the course text alone.

- Routes, layouts, and pages that should be indexed
- `metadata` / `generateMetadata` or `next/head`
- `robots`, sitemap, canonical, `noindex` on private or duplicate URLs
- Redirects and not-found behavior
- Headings and URL slugs
- Internal links versus raw `<a>` for in-app routes
- `<img>` versus `next/image`, font loading, `next/script`, large synchronous imports
- Any Web Vitals reporting already in the repo

Skip outreach and keyword research.

## Report

Open with what blocks crawling, indexing, or Core Web Vitals. Then one section per axis, in order from 01 to 09. Each checklist item is exactly one of:

- **corrigir** — file path and the change. In English replies, use **fix**.
- **ok** — file path that satisfies it
- **não se aplica** — why. In English replies, use **n/a**.

Close with what already matches the course, then what cannot be verified without field data (CrUX, Search Console, real users). Do not invent Lighthouse scores or ranking impact.
