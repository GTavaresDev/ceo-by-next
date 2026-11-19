# Axis 06: Popularity (Authority and Links)

Sources: NEST course, Chapter 1 (pillars), Chapter 15 (internal links and PageRank).

Popularity is the third pillar: boost the site's presence online so search engines see it as a trusted source. It works through links.

## 1. Backlinks (off-site)

- Backlinks are links from third-party sites back to your site (see axis 01).
- Search engines use them as a trust signal.
- Google started this principle with the PageRank algorithm. At a high level, PageRank scores domains based on **how many** links they receive (quantity) and **from which domains** (quality).
- Links from spam sites have little to no value. A link from a large national press site is likely very valuable.

## 2. Internal links (on-site)

- The internet is connected by links. Sites that receive more links tend to be seen as more trusted.
- Include links both internally (between your own pages) and externally (to other sites).
- Links must use `href` to count for PageRank calculations.

### next/link (Pages Router, from the course)

`Link` enables client-side transitions between routes. The `href` prop is required and puts the link on the anchor tag, which is vital for SEO. Googlebot can follow it without running JavaScript.

```jsx
import Link from 'next/link';

function NavLink({ href, name }) {
  return (
    <Link href={href}>
      <a>{name}</a>
    </Link>
  );
}

export default NavLink;
```

If the child of `Link` is a custom component that wraps an `a` tag, add `passHref` so the `a` tag receives the `href`:

```jsx
import Link from 'next/link';
import styled from 'styled-components';

const RedLink = styled.a`
  color: red;
`;

function NavLink({ href, name }) {
  return (
    <Link href={href} passHref>
      <RedLink>{name}</RedLink>
    </Link>
  );
}

export default NavLink;
```

Without `passHref`, the anchor has no `href` attribute, which hurts SEO. ESLint has a Next.js rule that catches this.

**Version note:** the `<a>` child pattern and `passHref` come from older Next.js versions. In Next.js 13 and later, `Link` renders the anchor itself. Verify against current docs.

## Key takeaways

- Popularity comes from links: quantity and quality of backlinks matter.
- Internal links help distribute authority and help crawlers and users move through the site.
- Every link needs a real `href`.

## Next.js checklist

- [ ] Navigation uses `next/link` with a real `href` on the rendered anchor.
- [ ] Custom link components pass `href` through (`passHref` on older versions).
- [ ] Pages link to related pages internally.
- [ ] Outreach for quality backlinks is planned (process pending from the course).
