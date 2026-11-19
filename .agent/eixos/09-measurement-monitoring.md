# Axis 09: Measurement and Monitoring

Sources: NEST course, Chapters 23 (Lighthouse), 29 (monitoring), 30 (Speed Insights), 31 (custom reporting), 32 (other tools), 33 (CrUX and dashboards).

Measure in two ways:

- **Lab data:** a simulated environment, repeatable, good for debugging (Lighthouse, Chrome DevTools).
- **Field data:** real users, good for monitoring over time (Speed Insights, CrUX, Search Console).

Track trends over time. One-off lab tests are not enough.

## 1. Lighthouse (Chapter 23)

Lighthouse runs audits on a URL and produces a report with suggestions.

How to run it (course method):

1. Open Chrome.
2. Open an **incognito** window and go to the page.
3. Open DevTools, go to the **Lighthouse** tab.
4. Click **Generate Report** (takes under 60 seconds).

Rules for accurate reports:

- Use incognito. Third-party extensions change results.
- Ad blockers can block scripts and make the audit incomplete. Use a clean persona if possible.
- **Always test a production build** (`npm run build && npm run start`). Development builds are not representative.

Course example: compare the optimized homepage of nextjs.org with an unoptimized sample app. Starter app:

```bash
npx create-next-app@latest nextjs-lighthouse --use-npm --example "https://github.com/vercel/next-learn/tree/main/seo"
cd nextjs-lighthouse
npm run build && npm run start
```

## 2. Lab tools (Chapter 32)

| Tool | Type | Use |
|------|------|-----|
| Lighthouse | Lab | Automated audits and suggestions |
| Chrome DevTools | Lab | Performance tools built into Chrome |

**Important:** FID can only be measured with field data. For lab tests, use **Total Blocking Time (TBT)** as the interactivity proxy.

## 3. Field tools (Chapter 32)

| Tool | Type | Use |
|------|------|-----|
| PageSpeed Insights | Field + lab | Google's page speed tool |
| Chrome User Experience Report (CrUX) | Field | Open dataset of real Chrome user metrics |
| Search Console, Core Web Vitals report | Field | Vitals for pages in Google Search |

## 4. Next.js Speed Insights (Chapter 30)

- Measures Core Web Vitals on real pages.
- Gives a "Real Experience Score" with zero configuration on Vercel deployments.

Note: the course calls this "Next.js Speed Insights". Verify the current product name and setup, since Vercel's docs may have changed.

## 5. Custom reporting with `reportWebVitals` (Chapter 31)

Next.js provides a `reportWebVitals` function that receives each metric. You can log it, send it to your own service, or push it to Google Analytics.

Pages Router (`pages/_app.js`):

```js
export function reportWebVitals(metric) {
  console.log(metric);
}
```

Build and start the app, then open DevTools. Metrics appear in the console. FID only fires after the first user interaction.

**Version note:** this is the Pages Router pattern. In the App Router, use the `useReportWebVitals` hook. Verify against current Next.js docs.

## 6. CrUX and dashboards (Chapter 33)

- The CrUX dataset is public on BigQuery and can be viewed in Google Data Studio at no cost.
- Open-source dashboards exist as templates to track your site's vitals.
- **Limit:** a site needs a meaningful number of visits to appear in CrUX. It is not useful for new or work-in-progress sites.
- **Delay:** data updates monthly, usually about 15 days after the month ends. It is not useful for fast feedback loops.

Note: Google Data Studio was replaced by Looker Studio. Verify the current tool.

## Key takeaways

- Use Lighthouse for repeatable lab tests on production builds, in incognito.
- Use field data (Speed Insights, CrUX, Search Console) to see what real users experience over time.
- For interactivity, use TBT in lab tests and FID or INP in field data.
- Send real metrics to your own analytics with `reportWebVitals` or the App Router hook.

## Next.js checklist

- [ ] Run Lighthouse on a production build in incognito before and after each optimization.
- [ ] Enable field monitoring for production (Speed Insights or equivalent).
- [ ] Check the Search Console Core Web Vitals report regularly.
- [ ] Send Web Vitals to your analytics with `reportWebVitals` (Pages Router) or `useReportWebVitals` (App Router).
- [ ] Track trends monthly. Do not rely only on one test.
