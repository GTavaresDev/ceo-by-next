# Axis 05: Creation (On-page SEO, URLs and Metadata)

Sources: NEST course, Chapter 1 (pillars), Chapter 13 (URL Structure), Chapter 14 (Metadata), Chapter 15 (On Page SEO: headings).

Creation covers what is on each page and how it is named: the URL, the title and description, the social and structured data, and the headings. Keyword targeting is the goal across all of these.

## 1. URL structure (Chapter 13)

Google does not disclose the weight of each SEO factor, but good URLs are a best practice either way.

Principles:

- **Semantic:** use words, not IDs or random numbers.
  - Better: `/learn/basics/create-nextjs-app`
  - Worse: `/learn/course-1/lesson-1`
- **Logical and consistent patterns:** group similar pages under one folder (for example, all products under `/products/`).
- **Keyword focused:** keywords help users and search engines understand the purpose of the page.
- **Not parameter-based:** parameters are rarely semantic, and search engines may confuse them and demote the results.

### Routes in Next.js (Pages Router, from the course)

File-system routing: a file added to `pages` becomes a route automatically.

| URL | File |
|-----|------|
| `/` | `pages/index.js` |
| `/products` | `pages/products.js` or `pages/products/index.js` |
| `/products/product` | `pages/products/product.js` |
| `/products/nextjs-shirt` (dynamic) | `pages/products/[product].js` |
| `/blog/seo-in-nextjs` (dynamic) | `pages/blog/[slug].js` |

Dynamic routes use brackets in the file name. Use the product ID or a readable slug.

## 2. Metadata (Chapter 14)

Metadata is the abstract of a page: title, description, image.

### Title

- The most important on-page SEO element for two reasons: users see it in search results, and Google uses it to understand the page.
- Include the main keywords and a clear value proposition.
- Example: `iPhone 12 XS Max For Sale in Colorado - Big Discounts | Apple`

### Description

- Less important than the title. Google says it is not used for ranking, but it affects click-through rate.
- Complement the title and add keywords that did not fit in it. Matching search terms appear in bold in results.

### Open Graph

- A protocol that standardizes how a page's metadata is shown when shared (social networks, WhatsApp, Telegram).
- Open Graph has no direct effect on rankings. It improves clickability and shares, which bring visitors.
- Twitter also has its own Twitter Cards tags.

### Structured data (JSON-LD)

- Helps search engines understand the page. The main vocabulary is schema.org.
- It can be encoded as RDFa, Microdata, or JSON-LD. JSON-LD is the common choice.
- Different search engines accept different parts of schema.org. Check which vocabularies each one supports.

### Next.js (Pages Router, from the course)

```jsx
import Head from 'next/head';

function IndexPage() {
  return (
    <div>
      <Head>
        <title>iPhone 12 XS Max For Sale in Colorado - Big Discounts | Apple</title>
        <meta
          name="description"
          content="Check out iPhone 12 XR Pro and iPhone 12 Pro Max. Visit your local store and for expert advice."
          key="desc"
        />
        <meta property="og:title" content="Social Title for Cool Page" />
        <meta property="og:description" content="And a social description for our cool page" />
        <meta property="og:image" content="https://example.com/images/cool-page.jpg" />
      </Head>
      <h1>iPhones for Sale</h1>
      <p>insert a list of iPhones for sale.</p>
    </div>
  );
}

export default IndexPage;
```

JSON-LD product data is added with a `script` tag inside `Head`:

```jsx
<script
  type="application/ld+json"
  dangerouslySetInnerHTML={{
    __html: JSON.stringify({
      '@context': 'https://schema.org/',
      '@type': 'Product',
      name: 'Executive Anvil',
      image: ['https://example.com/photos/1x1/photo.jpg'],
      description: 'Sleeker than ACME\'s Classic Anvil...',
      sku: '0446310786',
      brand: { '@type': 'Brand', name: 'ACME' },
      aggregateRating: { '@type': 'AggregateRating', ratingValue: '4.4', reviewCount: '89' },
      offers: {
        '@type': 'Offer',
        url: 'https://example.com/anvil',
        priceCurrency: 'USD',
        price: '119.99',
        availability: 'https://schema.org/InStock',
      },
    }),
  }}
  key="product-jsonld"
/>
```

The course hardcodes the JSON as a string. Building it from data with `JSON.stringify` keeps it dynamic.

## 3. Headings (Chapter 15)

- Headings show the structure of a page and help search engines find the most important parts.
- Levels run from H1 to H6. H1 is the most important.
- Use one H1 per page. It should describe the page and be similar to the `title` tag.

```jsx
function Page() {
  return <h1>Your Main Page Heading</h1>;
}
```

## Key takeaways

- URLs should be semantic, consistent, keyword-focused, and free of parameters.
- Titles are the most important metadata. Descriptions affect clicks. Open Graph affects sharing. JSON-LD helps search engines understand content.
- One H1 per page, aligned with the title.

## Next.js checklist

- [ ] Each page has a unique, keyword-focused `title` and `description` via `next/head`.
- [ ] Each page has exactly one `h1` that matches the title.
- [ ] URLs are semantic and follow a consistent pattern. Dynamic routes use readable slugs.
- [ ] Shareable pages include Open Graph tags (`og:title`, `og:description`, `og:image`).
- [ ] Product, article or other entity pages include relevant JSON-LD where the search engine supports it.
