# Sources

Materials used to build this agent. Links are from the course and Google's documentation as cited in it. Verify time-sensitive facts before relying on them.

## NEST course (Next.js SEO)

| Chapter | Topic | Used in |
|---------|-------|---------|
| 1 | Why is SEO so important? | `eixos/01-foundations.md` |
| 2 | Search Systems | `eixos/02-search-systems.md` |
| 3 | How Googlebot works (with pipeline diagram) | `eixos/02-search-systems.md` |
| 4 | Crawling and Indexing (intro) | `eixos/03-crawling-indexing.md` |
| 5 | HTTP status codes | `eixos/03-crawling-indexing.md` |
| 6 | robots.txt | `eixos/03-crawling-indexing.md` |
| 7 | XML sitemaps | `eixos/03-crawling-indexing.md` |
| 8 | Special meta tags for search engines | `eixos/03-crawling-indexing.md` |
| 9 | Canonical tags | `eixos/03-crawling-indexing.md` |
| 10 | Rendering and Ranking (intro) | `eixos/04-rendering-ranking.md` |
| 11 | Rendering strategies (SSG, SSR, ISR, CSR) | `eixos/04-rendering-ranking.md` |
| 12 | What about AMP? | `eixos/04-rendering-ranking.md` |
| 13 | URL structure | `eixos/05-creation.md` |
| 14 | Metadata (title, description, Open Graph, JSON-LD) | `eixos/05-creation.md` |
| 15 | On-page SEO (headings, internal links) | `eixos/05-creation.md`, `eixos/06-popularity.md` |
| 16 | Web performance and Core Web Vitals (intro) | `eixos/07-web-performance.md` |
| 17 | Web Vitals overview | `eixos/07-web-performance.md` |
| 18 | Largest Contentful Paint (LCP) | `eixos/07-web-performance.md` |
| 19 | First Input Delay (FID) | `eixos/07-web-performance.md` |
| 20 | Cumulative Layout Shift (CLS) | `eixos/07-web-performance.md` |
| 21 | SEO impact (page experience, Lighthouse weights, UX impact) | `eixos/07-web-performance.md` |
| 22 | Improving Core Web Vitals (overview) | `eixos/07-web-performance.md`, `eixos/08-optimization-techniques.md` |
| 23 | Introducing Lighthouse | `eixos/09-measurement-monitoring.md` |
| 24 | Automatic image optimization | `eixos/08-optimization-techniques.md` |
| 25 | Dynamic imports | `eixos/08-optimization-techniques.md` |
| 26 | Dynamic imports for components | `eixos/08-optimization-techniques.md` |
| 27 | Optimizing fonts | `eixos/08-optimization-techniques.md` |
| 28 | Optimizing third-party scripts | `eixos/08-optimization-techniques.md` |
| 29 | Monitoring Core Web Vitals | `eixos/09-measurement-monitoring.md` |
| 30 | Next.js Speed Insights | `eixos/09-measurement-monitoring.md` |
| 31 | Custom reporting | `eixos/09-measurement-monitoring.md` |
| 32 | Other tools | `eixos/09-measurement-monitoring.md` |
| 33 | Data Studio (CrUX) | `eixos/09-measurement-monitoring.md` |

## Referenced external resources (from the course)

- Google Search Console: https://search.google.com/search-console
- Google crawler overview: https://developers.google.com/search/docs/advanced/crawling/overview-google-crawlers
- Google sitemap guidance: https://developers.google.com/search/docs/advanced/sitemaps/overview
- Google robots.txt introduction: https://developers.google.com/search/docs/advanced/robots/intro
- Google page experience update: https://developers.google.com/search/blog/2021/04/more-details-page-experience
- Web Vitals: https://web.dev/vitals/
- Next.js SEO lessons: https://nextjs.org/learn/seo
- Next.js data fetching: https://nextjs.org/docs/basic-features/data-fetching
- Next.js routing: https://nextjs.org/docs/routing/introduction
- Smashing Magazine, ISR guide: https://www.smashingmagazine.com/2021/04/incremental-static-regeneration-nextjs
- Vercel, SSR vs static generation: https://vercel.com/blog/nextjs-server-side-rendering-vs-static-generation
- Stanford, PageRank paper: https://web.stanford.edu/class/cs54n/handouts/24-GooglePageRankAlgorithm.pdf
- MDN, User-Agent header: https://developer.mozilla.org/docs/Web/HTTP/Headers/User-Agent
- Moz, backlinks: https://moz.com/learn/seo/backlinks
- StatCounter, search engine market share: https://gs.statcounter.com/search-engine-market-share

## Corrections to the course (verify)

- FID was replaced by INP (Interaction to Next Paint) as a Core Web Vital in 2024.
- The `<a>` child pattern in `next/link` and `passHref` are from older Next.js versions. Next.js 13+ renders the anchor.
- AMP support was removed in Next.js 16, according to the course.
- Lighthouse weights (Ch. 21) are for v6. Check the current version.
- Font optimization (Ch. 27) is now handled by `next/font`.
- `reportWebVitals` (Ch. 31) is the Pages Router pattern. The App Router uses `useReportWebVitals`.
- Google Data Studio (Ch. 33) became Looker Studio.
- FID can only be measured in field data. Use TBT in lab tests.

## Gaps

- Course content is complete through Chapter 33.
- Creation and Popularity lessons on keyword research and link outreach have not been provided yet.
