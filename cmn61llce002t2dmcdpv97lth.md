---
title: "Sorting Hashnode Series Posts: How to Display the Latest Post First"
datePublished: 2026-03-25T12:51:27.951Z
cuid: cmn61llce002t2dmcdpv97lth
slug: sorting-hashnode-series-posts-how-to-display-the-latest-post-first
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/86b1a01d-5406-480d-87db-274576505e1d.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/ecac9ee7-eada-4723-b7ca-78837b3f0dde.png
tags: api, typescript, graphql

---

When you publish a series of articles on your Hashnode blog and consume it via their GraphQL API for a custom portfolio or website, you quickly run into a common roadblock: **Hashnode’s API natively returns series posts in chronological order (oldest first)**.

While this makes sense for consecutive tutorials ("Part 1", "Part 2"), it’s significantly less ideal for ongoing series—like my "Learning how to code with AI" series—where readers typically want to see your newest breakthroughs or the latest problem you solved directly at the top of the feed.

Unfortunately, looking at the Hashnode GraphQL Schema (`Series.posts`), there isn't an out-of-the-box `sort: RECENT` parameter available. Instead, it demands that you sequentially traverse the cursor paginations starting from the oldest post you've written in that series.

Here's how I solved this issue on my React/Vite portfolio to seamlessly deliver a newest-first reading experience to my users.

## The Problem: Oldest First Pagination

By default, the standard query to fetch series posts looks like this:

```graphql
query Series($host: String!, $slug: String!, $first: Int!, $after: String) {
  publication(host: $host) {
    series(slug: $slug) {
      posts(first: $first, after: $after) {
        edges {
          node {
            slug
            title
            publishedAt
          }
        }
        pageInfo { endCursor hasNextPage }
      }
    }
  }
}
```

This returns standard edge/node responses, but starting with the very first post of the series. To get the newest post to display on page 1 of our UI, we need the *end* of the dataset instead of the beginning.

## The Solution: A Client-Side Cache & Array Reversal

Since Hashnode API's `first` parameter maxes out at `20` items per request, we can't simply request `first: 1000` and reverse it in one go. We need to sequentially buffer the entire series.

To keep it blazing fast for users querying consecutive pages, we shouldn't fetch the whole thing from scratch every time they click "Next". Instead, we can introduce a local caching mechanism in our code.

We'll parse through all the posts via a `while` loop on the initial page load, cache the fully assembled series, `.reverse()` the data natively in JavaScript, and handle React's generic cursor logic over our local cache instead!

### Re-writing `fetchSeries` in TypeScript

Here's my updated `lib/hashnode.ts`:

```typescript
import { BlogPost, BlogSeries, PageInfo } from '../types';

// Let's declare local caches outside the function
const seriesMetaCache: Record<string, Omit<BlogSeries, 'posts'>> = {};
const seriesPostsCache: Record<string, BlogPost[]> = {};

export async function fetchSeries(
  slug: string,
  first = 9,
  after?: string
): Promise<{
  series: BlogSeries & { posts?: { edges: { node: BlogPost }[]; pageInfo: PageInfo } };
} | null> {

  // 1. If no 'after' is provided, we must be loading the series for the first time.
  // We'll hit the Hashnode API repeatedly until we traverse all oldest-first pages.
  if (!after || !seriesPostsCache[slug]) {
    let allPosts: BlogPost[] = [];
    let hasNextPage = true;
    let endCursor: string | undefined = undefined;
    let seriesMeta: Omit<BlogSeries, 'posts'> | null = null;

    while (hasNextPage) {
      const data: any = await gqlFetch(SERIES_QUERY, {
        host: PUBLICATION_HOST,
        slug,
        first: 20,
        after: endCursor
      });

      const publishedSeries = data.publication.series;
      if (!publishedSeries) return null;

      // Persist metadata on the first run
      if (!seriesMeta) {
        const { posts, ...metaRest } = publishedSeries;
        seriesMeta = metaRest;
      }

      const posts = publishedSeries.posts;
      allPosts = allPosts.concat(posts.edges.map((e: any) => e.node));

      hasNextPage = posts.pageInfo.hasNextPage;
      endCursor = posts.pageInfo.endCursor;
    }

    // 2. Here's the magic trick. Reverse the array so Newest is First.
    seriesPostsCache[slug] = allPosts.reverse();
    if (seriesMeta) {
      seriesMetaCache[slug] = seriesMeta;
    }
  }

  // 3. Grab from cache
  const allPosts = seriesPostsCache[slug];
  const seriesMeta = seriesMetaCache[slug];
  if (!seriesMeta) return null;

  // 4. Implement local slicing based on the provided cursors
  const startIndex = after ? allPosts.findIndex(p => p.slug === after) + 1 : 0;

  // Safety net in case of invalid cursor
  const validStartIndex = startIndex > 0 ? startIndex : (after ? allPosts.length : 0);

  const slice = allPosts.slice(validStartIndex, validStartIndex + first);
  const nextHasNextPage = validStartIndex + first < allPosts.length;

  // Set the newest 'endCursor' for the client to use on consecutive requests
  const newEndCursor = slice.length > 0 ? slice[slice.length - 1].slug : null;

  // 5. Structure exactly how the calling React component expects
  return {
    series: {
      ...seriesMeta,
      posts: {
        edges: slice.map(node => ({ node })),
        pageInfo: {
          endCursor: newEndCursor || '',
          hasNextPage: nextHasNextPage
        }
      }
    } as any
  };
}
```

## Why this approach is awesome:

1.  **Zero UI Adjustments**: The React component responsible for consuming the posts (`SeriesPage.tsx`) doesn’t even know this under-the-hood wizardry is occurring. It passes `first` and `after` standardly and receives seamless newest-first data.
    
2.  **Instant "Load More"**: Once the initial batch is traversed during `while(hasNextPage)`, it acts exactly like an instant static array. Any subsequent "Load More" clicks don't hit the standard network stack; they instantly slice through `seriesPostsCache` in micro-seconds!
    
3.  **Overcoming GraphQL Limitations**: Sometimes headless CMS or GraphQL servers (like Hashnode's otherwise fantastic API) restrict complex filtering or sorting on nested nodes purely for their own database performance. When you abstract that layer on your backend caching structure, you win full control back.
    

Now, whenever users visit the series pages in my portfolio, the most recently solved programming challenges are rightfully spotlighted at the very top. What's not to love about building digital experiences that slap?