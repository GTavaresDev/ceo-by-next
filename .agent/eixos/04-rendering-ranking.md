# Axis 04: Rendering and Ranking

Sources: NEST course, Chapter 10 (intro), Chapter 11 (Rendering Strategies), Chapter 12 (AMP).

## Why rendering matters

JavaScript is central to modern web development. Older sites sent all content from the server. With JavaScript, more content is fetched in the browser.

This affected search engines. Many bots only parsed the initial HTML sent by the server. If key content appears only after JavaScript runs, a bot that does not render may miss it. Googlebot can render, but rendering is costly and not guaranteed (see axis 02). Some other engines render less well than Google.

**The rule:** the most important thing for SEO is that page data and metadata are available on page load, without JavaScript. SSG or SSR are usually the best options.

## Rendering strategies

| Strategy | When HTML is generated | SEO fit | Best for |
|----------|------------------------|---------|----------|
| **SSG** (Static Site Generation) | At build time. The same HTML is served on each request. | Best. Full HTML on load, and fast, which helps performance (a ranking factor). | Blog posts, marketing pages, docs |
| **SSR** (Server-Side Rendering) | At request time. | Good. Pre-rendered HTML. | Very dynamic pages, news feeds |
| **ISR** (Incremental Static Regeneration) | Built statically, then created or updated per page after the build, without rebuilding the whole site. | Good. Static benefits at large scale. | Sites with millions of pages |
| **CSR** (Client-Side Rendering) | In the browser with JavaScript. The server sends a near-empty HTML file first. | Not recommended for SEO. | Data-heavy dashboards, account pages, pages that should not be indexed |

- Each method can be chosen **per page** in Next.js. Example: blog posts as SSG, a customer account dashboard as CSR, a news feed as SSR.
- ISR lets content editors update static pages without a full rebuild.

## AMP (Chapter 12)

- Google once gave ranking preference to AMP pages (a technology for faster mobile pages), at the cost of building and maintaining them.
- The Core Web Vitals page experience update dropped AMP as a requirement for search carousels. This removed one of the last main SEO benefits of AMP.
- **Next.js 16 removed AMP support.** For new projects, optimize Core Web Vitals directly with Next.js's built-in performance features (see axis 07).
- For legacy projects on Next.js 15 or earlier that need AMP, refer to the legacy AMP documentation.

## Key takeaways

- Search engines do not always run JavaScript, so server-delivered content is safer.
- SSG is the best default for SEO. SSR suits dynamic pages. ISR scales static pages. CSR is for pages that do not need to rank.
- Rendering strategy is a per-page decision in Next.js.
- AMP is no longer a ranking requirement and is not supported in Next.js 16.

## Next.js checklist

- [ ] Search-critical pages send their main content and metadata in the initial HTML (SSG, ISR or SSR).
- [ ] Use CSR only for pages that should not be indexed or do not need to rank.
- [ ] Choose ISR when a site has too many pages to build at once.
- [ ] Do not add AMP to new projects.

## Next.js reference (from the course)

```js
// SSG with getStaticPaths and fallback: 'blocking'
// pages/blog/[slug].js
export async function getStaticPaths() {
  const res = await fetch('https://www.example.com/api/posts');
  const posts = await res.json();
  const paths = posts.map((post) => ({ params: { slug: post.slug } }));
  // Pages not built at build time are rendered on first request, then cached as static.
  return { paths, fallback: 'blocking' };
}
```

Note: the Pages Router examples come from the course. Verify against current docs before using them in an App Router project.
