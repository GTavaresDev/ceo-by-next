# Axis 08: Optimization Techniques

Sources: NEST course, Chapters 22 (overview), 24 (images), 25 (dynamic imports), 26 (dynamic component imports), 27 (fonts), 28 (third-party scripts).

These techniques improve Core Web Vitals in Next.js. Measure before and after with Lighthouse (axis 09).

## 1. Images with `next/image` (Chapter 24)

### Problem with plain HTML

```html
<img src="large-image.jpg" alt="Large Image" />
```

Plain `img` tags require you to handle these yourself:

- Responsive sizes for different screens
- Compression and modern formats
- Lazy loading as images enter the viewport

### What `next/image` does

- Extends the HTML `img` element.
- Resizes, optimizes and serves modern formats such as WebP when the browser supports them.
- Avoids sending large images to small viewports.
- **On-demand optimization:** images are optimized when requested, not at build time. Build times do not grow with the number of images.
- **Lazy loading by default:** images load only when they enter the viewport.
- **Avoids CLS:** images are rendered with reserved space.
- Works with any image source, including external CMS images.

### Usage

```jsx
import Image from 'next/image';

// Before
<img src="large-image.jpg" alt="Large Image" />

// After
<Image src="/large-image.jpg" alt="Large Image" width={3048} height={2024} />
```

- `width` and `height` should be the rendered size, with the same aspect ratio as the source image.
- The footer logo from the course: `<Image src="/vercel.svg" alt="Vercel Logo" width={72} height={16} />`

## 2. Dynamic imports for libraries (Chapter 25)

Dynamic `import()` (ES2020) loads a module only when it is needed. It also works with SSR. Think of it as code splitting.

Before, the page imported heavy libraries at load time:

```js
import Fuse from 'fuse.js';
import { now } from 'lodash-es';
```

After, the libraries load on the first search input:

```jsx
<input
  type="text"
  placeholder="Country search..."
  className={styles.input}
  onChange={async (e) => {
    const { value } = e.currentTarget;

    // Load libraries on demand
    const Fuse = (await import('fuse.js')).default;
    const { now } = await import('lodash-es');

    const fuse = new Fuse(countries, { keys: ['name'], threshold: 0.3 });
    const searchResult = fuse.search(value).map((result) => result.item);
    const updatedResults = searchResult.length ? searchResult : countries;
    setResults(updatedResults);

    console.info({ searchedAt: now() });
  }}
/>
```

Result in the course: fixes the "Remove unused JavaScript" audit on load and improves Time to Interactive (TTI), which supports FID.

## 3. Dynamic imports for components (Chapter 26)

Use `next/dynamic` for React components that are not needed on first load, such as modals.

```jsx
import dynamic from 'next/dynamic';

const CodeSampleModal = dynamic(() => import('../components/CodeSampleModal'), {
  ssr: false, // not needed on the server
});

// Render only when opened. The JavaScript is requested on first open.
{isModalOpen && (
  <CodeSampleModal isOpen={isModalOpen} closeModal={() => setIsModalOpen(false)} />
)}
```

Note: the course removes two third-party libraries from the initial load with this change.

## 4. Web fonts (Chapter 27)

- About 82% of desktop web pages use web fonts (course figure, 2020).
- Next.js optimizes web font loading by default. It inlines the font CSS at build time, which removes an extra network round trip for font declarations.
- Benefits: better First Contentful Paint (FCP) and Largest Contentful Paint (LCP).

Before, an extra request is needed:

```html
<link href="https://fonts.googleapis.com/css2?family=Inter" rel="stylesheet" />
```

After, Next.js inlines the CSS:

```html
<style data-href="https://fonts.googleapis.com/css2?family=Inter">
  @font-face { font-family: 'Inter'; font-style: normal; }
</style>
```

**Version note:** the course describes build-time inlining. Current Next.js handles fonts with `next/font`. Verify the current API before using it.

## 5. Third-party scripts (Chapter 28)

Third-party scripts (analytics, ads, support widgets) can delay rendering if loaded too early.

Plain HTML requires manual placement in `next/head`:

```jsx
import Head from 'next/head';

<Head>
  <script src="https://www.googletagmanager.com/gtag/js?id=123" />
</Head>
```

With the `Script` component, you can place it anywhere and choose when it loads with `strategy`:

```jsx
import Script from 'next/script';

<Script
  strategy="afterInteractive"
  src="https://www.googletagmanager.com/gtag/js?id=123"
/>
```

- `afterInteractive`: loads right after the page becomes interactive. Good default for analytics.
- `lazyOnload`: loads during browser idle time. Good for low-priority scripts such as chat widgets.
- Defer most third-party scripts so they do not hurt LCP.

## Key takeaways

- Use `next/image` for images. It handles resizing, modern formats, lazy loading and CLS.
- Use dynamic imports for libraries and components that are not needed on first load.
- Next.js inlines font CSS to reduce round trips.
- Load third-party scripts after the page is interactive or during idle time.

## Next.js checklist

- [ ] Replace `<img>` with `next/image` and set `width` and `height` to the rendered size.
- [ ] Find heavy libraries in the initial bundle. Load them with `import()` when needed.
- [ ] Use `next/dynamic` for modals and other components not needed on first render.
- [ ] Use Next.js font optimization for web fonts.
- [ ] Use `next/script` with `afterInteractive` or `lazyOnload` for third-party scripts.
- [ ] Re-run Lighthouse after each change and compare.
