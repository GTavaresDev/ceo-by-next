# Axis 03: Crawling and Indexing

Source: NEST course, Chapters 4 to 9.

## Overview

This axis covers what controls crawling and indexing:

- HTTP status codes
- `robots.txt`
- XML sitemaps
- Meta robots and Google-specific meta tags
- Canonical tags

**Directive vs suggestion:** `robots.txt` rules and meta robots tags are **directives** that search engines always obey. Canonical tags are **recommendations** that Google may or may not follow.

---

## 1. HTTP status codes

Status codes show whether a request succeeded. Only some matter for SEO.

| Code | Meaning | SEO action |
|------|---------|------------|
| **200 OK** | Request succeeded | Required for a page to be indexed. This is the default when Next.js renders a page successfully. |
| **301 Moved Permanently** | Permanent redirect | Use for moved content so users and bots reach the new URL. The most widely used redirect. |
| **308 Permanent Redirect** | Permanent redirect | Next.js uses 308 by default for permanent redirects. Unlike 301, it keeps the request method (it does not change POST to GET). |
| **302 Found** | Temporary redirect | Use only for temporary moves, such as a promotion page or location-based redirects. In most cases, a 301 is the right choice. |
| **404 Not Found** | Resource not found | Fine for URLs that do not exist. Should not be frequent, since it can hurt rankings. Moved pages should use 301 instead. |
| **410 Gone** | Resource removed permanently | Tells bots never to return. Use for content deleted for good (discontinued products, deleted threads, removed blog posts). |
| **500 Internal Server Error** | Unexpected server error | Next.js returns this for unexpected application errors. |
| **503 Service Unavailable** | Server not ready | Use when the site is down for an extended time. Helps avoid long-term ranking loss. |

### Next.js examples (Pages Router, from the course)

Permanent redirect from `getStaticProps` (triggers 308):

```js
// pages/about.js
export async function getStaticProps(context) {
  return {
    redirect: {
      destination: '/',
      permanent: true, // triggers 308
    },
  };
}
```

Permanent redirect in `next.config.js`:

```js
// next.config.js
module.exports = {
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true, // triggers 308
      },
    ];
  },
};
```

Return a 404 from a page:

```js
export async function getStaticProps(context) {
  return {
    notFound: true, // triggers 404
  };
}
```

Custom error pages, statically generated at build time:

```jsx
// pages/404.js
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>;
}

// pages/500.js
export default function Custom500() {
  return <h1>500 - Server-side error occurred</h1>;
}
```

---

## 2. robots.txt

A `robots.txt` file tells crawlers which pages or files they can or cannot request. Most good bots read it before requesting anything from a domain.

Use it to keep crawlers out of areas such as the CMS or admin, user accounts in an e-commerce site, or some API routes.

**Rules:**

- It must be served at the root of each host (`/robots.txt`). Alternatively, redirect the root path to a destination URL.
- `robots.txt` blocks crawling, not indexing. If a page was already indexed before the rule was added, it may stay indexed. Use `noindex` to remove a page from the index.

**Next.js setup:** place a `robots.txt` file in the `public` folder. The folder name `public` cannot be changed; it is the only directory used to serve static assets. The `public` name is not part of the URL.

```text
# public/robots.txt

# Block all crawlers from /accounts
User-agent: *
Disallow: /accounts

# Allow all crawlers
User-agent: *
Allow: /
```

Note: when you run `yarn dev`, the file is available at `http://localhost:3000/robots.txt`.

---

## 3. XML sitemaps

Sitemaps are the easiest way to communicate with Google. They list the site's URLs and when they were updated, so crawlers can find new content and crawl more efficiently.

Sitemaps can also be RSS, Atom, or plain text files. XML is the most common.

Sitemaps are not mandatory, but they help crawlers find and index content faster. Dynamic sitemaps are recommended, because they update as content changes. Static sitemaps are valid but less useful for ongoing discovery. For criteria on when to use one, see axis 02.

### Option A: manual (simple, static sites)

Create `public/sitemap.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>http://www.example.com/foo</loc>
    <lastmod>2021-06-01</lastmod>
  </url>
</urlset>
```

### Option B: dynamic (sites with changing content)

Create `pages/sitemap.xml.js`. It fetches the URLs of dynamic pages and writes an XML response for `/sitemap.xml`. The example below is from the course and uses `getServerSideProps`:

```js
// pages/sitemap.xml.js
const EXTERNAL_DATA_URL = 'https://jsonplaceholder.typicode.com/posts';

function generateSiteMap(posts) {
  return `<?xml version="1.0" encoding="UTF-8"?>
    <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
      <!-- We manually set the two URLs we know already -->
      <url>
        <loc>https://jsonplaceholder.typicode.com</loc>
      </url>
      <url>
        <loc>https://jsonplaceholder.typicode.com/guide</loc>
      </url>
      ${posts
        .map(({ id }) => {
          return `
        <url>
          <loc>${`${EXTERNAL_DATA_URL}/${id}`}</loc>
        </url>
      `;
        })
        .join('')}
    </urlset>
  `;
}

function SiteMap() {
  // getServerSideProps does the work
}

export async function getServerSideProps({ res }) {
  // Fetch the URLs of the site
  const request = await fetch(EXTERNAL_DATA_URL);
  const posts = await request.json();

  // Generate the XML sitemap
  const sitemap = generateSiteMap(posts);

  res.setHeader('Content-Type', 'text/xml');
  res.write(sitemap);
  res.end();

  return { props: {} };
}

export default SiteMap;
```

---

## 4. Meta robots tags

Meta robots tags are directives that search engines always respect. They are page-level and can be changed on demand, which `robots.txt` cannot do per page.

**Default:** `index,follow`. You do not need to specify it. `all` is an equivalent value.

```html
<meta name="robots" content="noindex,nofollow" />
```

| Directive | Effect |
|-----------|--------|
| `noindex` | Do not show this page in search results. Common for settings pages, internal search pages, and policies. |
| `nofollow` | Do not follow links on this page. Links may still be discovered from other pages. |

**Conflicting tags:** the more restrictive tag applies.

**Use case:** if a product filter returns no results, it is common practice to `noindex` that page.

**Note:** Google may occasionally index a page without crawling it, when it is certain the page matches what users expect. This is rare.

### Googlebot tag

```html
<meta name="googlebot" content="noindex,nofollow" />
```

Use it to set a rule only for Googlebot. Use `robots` for all other bots. In most cases, `robots` is enough.

### Google-specific tags

- `nositelinkssearchbox`: stops Google from showing the site search box in results.
- `notranslate`: tells Google not to offer a translation of the page. Useful when the content should not be shown translated.

```html
<meta name="google" content="nositelinkssearchbox" />
<meta name="google" content="notranslate" />
```

### Next.js example with `next/head`

```jsx
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <title>Meta Tag Example</title>
        <meta name="google" content="nositelinkssearchbox" key="sitelinks" />
        <meta name="google" content="notranslate" key="notranslate" />
      </Head>
      <p>Here we show some meta tags off!</p>
    </div>
  );
}

export default IndexPage;
```

`next/head` appends elements to the page `<head>`. Use the `key` prop so a tag is rendered only once if it is set in more than one place.

---

## 5. Canonical tags

A canonical URL is the URL that search engines consider the most representative of a set of duplicate pages.

- Search engines may group duplicate URLs on their own, for example when the same content is reachable under several paths.
- Duplicate content can lead to demotion. This also applies across domains.
- Canonical tags tell Google which URL is the source of truth.
- Canonical tags are recommendations. Google may decide otherwise.

**Common causes of duplicates:**

- The same product under two paths, such as `/products/phone` and `/phone`.
- UTM parameters added by marketing campaigns, which create new indexable URLs.

```html
<link rel="canonical" href="https://example.com/products/phone" />
```

**Next.js example:**

```jsx
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <title>Canonical Tag Example</title>
        <link
          rel="canonical"
          href="https://example.com/blog/original-post"
          key="canonical"
        />
      </Head>
      <p>This post exists on two URLs.</p>
    </div>
  );
}

export default IndexPage;
```

---

## Key takeaways

- Return **200** for pages you want indexed. Use **301/308** for permanent moves, **302** only for temporary ones, **410** for permanently deleted content, and **503** for long outages.
- `robots.txt` controls crawling. `noindex` controls indexing. Do not rely on `robots.txt` alone to de-index a page.
- Use a sitemap for large, new, poorly linked, or media-heavy sites. Make it dynamic when content changes often.
- Use canonical tags to consolidate duplicate URLs, including UTM variants.

## Next.js checklist

- [ ] Important pages return 200. Moved pages use 301/308 redirects. Deleted pages return 404 or 410.
- [ ] `public/robots.txt` blocks private areas (admin, accounts, API routes where relevant).
- [ ] `sitemap.xml` exists (static in `public/`, or dynamic in `pages/sitemap.xml.js`).
- [ ] Pages that should not appear in search have `noindex`.
- [ ] Every page with duplicate URLs declares a canonical link using `next/head`.
- [ ] Use `next/head` `key` props to avoid duplicated meta tags.
