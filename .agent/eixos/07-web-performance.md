# Axis 07: Web Performance and Core Web Vitals

Sources: NEST course, Chapters 16 to 20.

Web Vitals is a Google initiative with unified metrics for user experience on the web. Core Web Vitals is the subset of three metrics that Google uses as a ranking factor. Poor scores can hurt traffic and business.

Core Web Vitals measure:

- **Loading** (LCP)
- **Interactivity** (FID in the course; see the version note below)
- **Visual stability** (CLS)

## Scoring thresholds

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| **LCP** (Largest Contentful Paint) | ≤ 2.5 s | 2.5 s to 4.0 s | > 4.0 s |
| **FID** (First Input Delay) | ≤ 100 ms | 100 ms to 300 ms | > 300 ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | 0.1 to 0.25 | > 0.25 |

Two ways to approach the metrics:

- Aim for the best possible score on each metric. This can be hard on large sites with many dependencies.
- Benchmark against competitors. You compete with the sites that rank for your target keywords, not with every optimized site on the web.

## Chapter 18: LCP (Largest Contentful Paint)

- Measures how long the largest visible element in the viewport takes to appear. That element can be a text block, image, or video.
- It is **not** First Contentful Paint (FCP), which measures when the first element appears.
- The largest element can change as the DOM renders. LCP keeps counting until the largest element is on screen.

## Chapter 19: FID (First Input Delay)

- Measures the user's perception of responsiveness. Example: clicking an input and nothing happens.
- Happens when the browser's main thread is busy with other tasks and cannot respond to input.
- FID needs real user data and cannot be measured in the lab (for example, Lighthouse). **Total Blocking Time (TBT)** is the lab-measurable metric that captures the same interactivity issues.

## Chapter 20: CLS (Cumulative Layout Shift)

- Measures how much the layout moves after it first renders. Unexpected shifts cause misclicks and distraction.
- Example: a button renders above a text block and pushes it down.
- The score combines the impact and the distance of each shift.
- Only unexpected movement counts. Adding a new element, or changing the size of one, does not count if existing elements keep their positions.

## Version note (verify before use)

The course is older. Google replaced FID with **INP (Interaction to Next Paint)** as the Core Web Vital for interactivity in 2024. The FID thresholds above are from the course, so check current Google documentation before reporting scores.

## Key takeaways

- Core Web Vitals are LCP, interactivity, and CLS. They are a ranking factor and a user experience measure.
- Lab tools (Lighthouse) help during development. Real user data shows what users experience.
- Reduce main-thread work for interactivity, reserve space for elements to avoid layout shift, and make the largest element load fast.

## Next.js checklist

- [ ] Measure Core Web Vitals in both lab (Lighthouse) and field (real user) data.
- [ ] Load the largest above-the-fold element quickly (optimize images, avoid blocking resources).
- [ ] Reserve space for images, ads and embeds so the layout does not shift.
- [ ] Reduce JavaScript on the main thread so pages respond to input quickly.
- [ ] Monitor changes over time after each release.

## SEO impact (Chapter 21)

- Google's goal is to give users the best results, with fast and smooth experiences. Mobile page speed has been a ranking factor since 2018.
- In June 2021, Google published specific metrics and ranges for page experience. Before that, the exact performance metrics used for ranking were unclear.
- Core Web Vitals are one group of **search signals for page experience**. The others are:
  - Mobile friendly
  - Safe browsing
  - HTTPS
  - No intrusive interstitials
- Google's ranking impact is the same for all pages in the "good" range for all Core Web Vitals, regardless of their individual scores.

### Lighthouse weights for the vitals (v6, verify current version)

| Lighthouse metric | Weight |
|-------------------|--------|
| Largest Contentful Paint | 25% |
| Total Blocking Time | 25% |
| First Contentful Paint | 15% |
| Speed Index | 15% |
| Time to Interactive | 15% |
| Cumulative Layout Shift | 5% |

The three vitals are not weighted equally. Lighthouse weights change between versions, so check the current weights before relying on them.

### UX impact

- Core Web Vitals are designed to improve page experience and ranking, but users are the ones who benefit.
- A 2012 Amazon study estimated that one extra second of load time could cost about 1.6 billion USD in sales (historical figure from the course).

## Improving Core Web Vitals (Chapter 22: overview)

The next lessons show how to improve the vitals of an example app with Next.js features. They are covered in axes 08 (optimization techniques) and 09 (measurement and monitoring).

## Field data example (illustrative)

The course includes a CrUX chart of monthly LCP distribution across sites, from Sep 2019 to Apr 2020. The share of "good" LCP (under 2,500 ms) fell from 68.85% in September 2019 to 56.04% in April 2020. In the same period, the 75th percentile (P75) LCP rose to 3,800 ms, and "poor" (4,000 ms or more) rose to 23.66%. Use this as an example of how field data shows trends over time, not as current figures.
