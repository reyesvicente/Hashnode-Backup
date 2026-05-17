---
title: "How I Fixed the Hashnode GraphQL API Stale Cache Bug (Stellate CDN)"
datePublished: 2026-02-23T05:27:26.078Z
cuid: cmlyqj0cc006627lvguola3gg
slug: how-i-fixed-the-hashnode-graphql-api-stale-cache-bug-stellate-cdn
cover: https://cloudmate-test.s3.us-east-1.amazonaws.com/uploads/covers/5f95116240346172a86c20c6/9c8b547a-834d-4b48-9b0c-39f3ca8dd82a.png
ogImage: https://cloudmate-test.s3.us-east-1.amazonaws.com/uploads/og-images/5f95116240346172a86c20c6/7a1164e3-5483-49e0-abbb-dc9c36fab2c8.png
tags: web-development, debugging, reactjs, graphql

---

If you're using Hashnode's GraphQL API to fetch your blog posts for a custom frontend (like a React or Next.js portfolio), you've probably run into this incredibly frustrating issue: **You publish a new post on Hashnode, but it doesn't show up on your website.**

You check the API payload, and it's serving a stale list of posts. The new post is completely missing.

I spent hours debugging this, trying every cache-busting trick in the book. Here's what didn't work, why it failed, and the actual simple solution.

## **The Setup**

My portfolio runs on React (Vite) and uses Hashnode as a headless CMS. I fetch posts using `fetch` with the official Hashnode GraphQL endpoint: `https://gql.hashnode.com`.

```typescript
const res = await fetch('https://gql.hashnode.com', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ query: POSTS_QUERY, variables }),
});
```

When I added a new post, my local dev server still only showed the old posts.

## **The Failed Attempts**

Hashnode uses **Stellate**, a powerful GraphQL Edge CDN. Stellate sits between your frontend and Hashnode's database, caching responses to make the API blazing fast. However, its caching mechanism is exceptionally aggressive.

Here are the standard tricks I tried to bypass the cache, all of which **failed**:

1.  **Setting standard HTTP headers:** Adding `Cache-Control: no-cache` and `Pragma: no-cache` to the `fetch` call. Stellate ignores these from the client.
    
2.  **URL Query Params:** Appending a timestamp `?_t=${Date.now()}` to the endpoint URL. Since this is a `POST` request, Stellate keys the cache off the request body, not just the URL.
    
3.  **GraphQL** `extensions`**:** Adding `extensions: { cacheKey: Date.now() }` to the JSON body. Stellate normalization strips this out.
    
4.  **Unknown GraphQL Variables:** Injecting `_cacheBust: Date.now()` into the `variables` object. Stellate strips unknown variables.
    
5.  **GraphQL Comments:** Injecting `# ${Date.now()}` directly into the query string. Stellate parses and normalizes the query string, stripping comments before hashing the cache key.
    

No matter what I did, inspecting the network tab always revealed the same mocking response header: `gcdn-cache: HIT`.

## **The Real Issue: Stellate Needs the** `id` **Field**

The root cause isn't that Stellate is ignoring your cache-busting hacks; it's that **Stellate doesn't know the data is stale.**

Stellate automatically invalidates its cache when backend data changes (mutations). But to do this effectively across complex GraphQL graphs, it relies on a core concept: **tracking Node IDs**.

If your query doesn't ask for the `id` of the entities it's fetching, Stellate cannot trace the cached data back to the actual database records. When Hashnode updates its database, Stellate's purge mechanism fires, but if your cached query didn't include IDs, Stellate doesn't know to invalidate *that specific query*.

This is a well-known issue internally at Hashnode — they even created an ESLint plugin (`require-id-when-available`) for their own engineers to prevent it!

## **The Solution**

The fix is almost disappointingly simple. **You must include the** `id` **field in all your GraphQL queries and fragments related to Hashnode.**

My original query looked like this:

```graphql
query Publication($host: String!, $first: Int!) {
  publication(host: $host) {
    posts(first: $first) {
      edges {
        node {
          slug
          title
          brief
          publishedAt
        }
      }
    }
  }
}
```

The fix is simply adding `id` inside the `node`:

```graphql
query Publication($host: String!, $first: Int!) {
  publication(host: $host) {
    id  # <-- ESSENTIAL FOR LIST INVALIDATION
    posts(first: $first) {
      edges {
        node {
          id  # <-- ESSENTIAL FOR ENTITY INVALIDATION
          slug
          title
          brief
          publishedAt
        }
      }
    }
  }
}
```

Make sure to add `id` to everywhere you query entities: the parent `publication` object, `posts`, `post`, `seriesList`, etc.

**Why both?**

*   Adding `id` to the `node` tells Stellate to update the cache for that specific post if it gets edited.
    
*   Adding `id` to the parent `publication` tells Stellate to invalidate the *entire list* of posts for that publication when a new post is published or deleted.
    

Once I updated my queries and refreshed, the CDN properly registered the unique records and lists. New posts now invalidate the cache automatically, and the API returns fresh data immediately upon publishing.

**Takeaway:** When working with GraphQL APIs behind Edge CDNs like Stellate, always fetch the `id`. It's not just good practice; it's the anchor for their entire caching strategy.