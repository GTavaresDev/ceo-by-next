# Axis 02: Search Systems and Googlebot

Source: NEST course, Chapters 2 and 3, and the "How Does Googlebot Work?" diagram.

## Search systems

Search systems (Google, Bing, DuckDuckGo, etc.) are large, complex systems. They have four main responsibilities:

| Responsibility | What it does |
|----------------|--------------|
| **Crawling** | Goes through the web and parses the content of websites. The scale is huge: there are over 350 million domains. |
| **Indexing** | Finds places to store the gathered data so it can be accessed later. |
| **Rendering** | Executes resources such as JavaScript that enhance features and enrich content. Not every crawled page is rendered. Rendering can happen before indexing, or after it if resources are not available at crawl time. |
| **Ranking** | Queries the stored data and builds results pages that best match user intent, applying ranking criteria. |

## Googlebot

Googlebot is Google's web crawler. It gathers the information needed to build the database that serves search results.

- Search engines use web crawlers (bots) that emulate users and follow links to discover and index pages.
- Crawlers identify themselves with a user-agent string.
- Google has several crawlers. The most used are **Googlebot Desktop** and **Googlebot Smartphone**.
- Other engines (Bing, Yandex, Baidu, Naver, Yahoo, DuckDuckGo) work similarly for crawling and indexing. They differ more in ranking and rendering.
- Google is the largest engine in most countries. Check other engines' guidelines if your audience is in markets where they lead, such as China, Russia, Japan or South Korea.

## How Googlebot works (pipeline)

Based on the diagram "How Does Googlebot Work?":

```
Crawl Queue → Crawler → Processing → Index
     ↑            (URLs / HTML)   │
     └────────────────────────────┘ (new URLs found)
                                   │
                            Render Queue → Renderer → Rendered HTML → Processing
```

1. **Find URLs.** Google gets URLs from several sources: Google Search Console, links between websites, and XML sitemaps.
2. **Add to Crawl Queue.** URLs wait in the crawl queue. Usually this takes seconds, but it can take a few days, especially for pages that need rendering, indexing, or refreshing.
3. **HTTP request.** The crawler requests the URL and acts on the status code (see axis 03).
4. **Render Queue.** The HTML is processed and content is parsed. If a page relies on client-side JavaScript, it may be sent to the Render Queue. Rendering is expensive for Google, so only a small share of pages is rendered. Other engines may not render JavaScript as well as Google, which is why the rendering strategy matters (see axis 04).
5. **Ready to be indexed.** If all criteria are met, the page can be indexed and shown in results.

## Key takeaways

- A search system has four jobs: crawl, index, render, rank.
- Googlebot discovers URLs, queues them, requests them, and may render them before indexing.
- JavaScript-heavy pages depend on the Render Queue, which is slower and not guaranteed. Server-rendered HTML is safer.
- Your site is only found if the crawler can reach it. Sitemaps and internal links help discovery.

## Sitemap guidance from Google (summary)

**A sitemap is likely needed when:**

- The site is large, so some pages may not be linked from another page.
- The site is new and has few external links, so crawlers may not find it.
- The site has a lot of rich media (video, images) or appears in Google News.

**A sitemap may not be needed when:**

- The site is small (up to about 500 pages that you want in search results).
- Internal links connect all important pages from the home page.
- The site has little media and no news content to surface.

## Next.js checklist

- [ ] Decide whether the site needs a sitemap, using the criteria above (see axis 03).
- [ ] Make sure important pages are linked from other pages, not only from client-side navigation.
