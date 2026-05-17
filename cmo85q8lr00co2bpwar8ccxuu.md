---
title: "Renaming 1000+ Pages Worth of "Levels" Without Losing My Mind"
datePublished: 2026-04-21T05:02:17.874Z
cuid: cmo85q8lr00co2bpwar8ccxuu
slug: renaming-1000-pages-worth-of-levels-without-losing-my-mind
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/89a6ae9f-9848-43b7-a8cc-5fde853cba10.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/c90e4d1e-c0c7-458a-a1ba-4a697176b10a.png
tags: software-development, wordpress, python, automation

---

A message from a colleague dropped into my inbox one morning:

> Hi Ice, now that we pushed the changes sa site, we need to scour the site for mentions of Level 1, Level 2, Level 1/2 and change them accordingly to Level 1 → Intro to Neurofascial Training, Level 1/2 → Intermediate Instability Training, Level 2 → Advanced Neurofascial Training

On the surface, this looks like a simple find-and-replace job. Read the message, open the WordPress admin, use the search feature, fix each page. Done before lunch.

Then I actually opened the site and pulled the sitemap. **1,074 URLs.**

That changes the math pretty quickly. Even if only a fraction of those pages mention "Level" anywhere, manually clicking through every post and page, ctrl-F-ing for three different strings, and then figuring out which instance needs which replacement — that's a full day of soul-crushing work at minimum, and a very real chance of missing something.

## **The mapping**

Before anything else, I wrote the rename table down somewhere I couldn't lose it, because a misread mapping here would mean renaming correct text into incorrect text, which is strictly worse than leaving it alone:

| Old text | New text |
| --- | --- |
| Level 1 | Intro to Neurofascial Training |
| Level 1/2 | Intermediate Instability Training |
| Level 2 | Advanced Neurofascial Training |

The important ordering detail: "Level 1/2" has to be matched *before* "Level 1", or a naive find-and-replace would turn "Level 1/2" into "Intro to Neurofascial Training/2". Easy to miss, painful to undo.

## **Why not just use WordPress search-replace plugins?**

Plugins like Better Search Replace do exist, and in a simpler world I'd use one. But they operate on the database directly, which means:

1.  One wrong checkbox and you nuke every "Level 1" across post content, post meta, widget text, theme options, and anywhere else the phrase appears — including places it *shouldn't* be replaced, like analytics or audit logs.
    
2.  There's no preview of *where* each match lives. You're trusting the plugin's count and hoping nothing weird is hiding in a shortcode.
    
3.  The three strings overlap, so the order of operations matters and plugins don't always let you sequence replacements atomically.
    

What I actually wanted was a map — a list of every occurrence with enough surrounding context to judge whether it's safe to change, plus a direct link to the WordPress editor for that specific page. The replacement itself I'd still do by hand, but informed by real data.

## **The script**

I wrote a Python script that walks the site's sitemap, visits each URL, and searches the rendered HTML for the three patterns using a single regex with word boundaries:

```python
LEVEL_PATTERN = re.compile(
    r"\bLevel\s*1\s*/\s*2\b|\bLevel\s*1\b|\bLevel\s*2\b",
    re.IGNORECASE,
)
```

Word boundaries matter here. Without `\b`, a page mentioning "Level 10" or "Level 12-week program" would get flagged as a false "Level 1" match. And the alternation order mirrors the mapping problem above — "Level 1/2" is tried first so it doesn't get shadowed by the simpler "Level 1" pattern.

For each match, the script captures:

*   The page URL and `<title>`
    
*   The WordPress post ID, extracted from the `<body>` class (WP adds `page-id-123` or `postid-123` automatically)
    
*   A direct admin edit link built from that ID: `/wp-admin/post.php?post=123&action=edit`
    
*   About 80 characters of context on either side of the match
    
*   The HTML tag wrapping the match (`h2`, `li`, `p`, etc.) to help me find it inside the block editor
    

All of it dumped to a CSV. Rows sorted, duplicates within a page collapsed, system paths like `/wp-admin` and `/wp-json` filtered out so the scanner doesn't waste time on pages that aren't user content.

## **What the CSV actually gives me**

Instead of 1,074 pages to click through blindly, I now have a focused list of every page that mentions "Level" in any form, with a one-click link to the editor and enough context to know which replacement to apply. The context column is the real magic — I can see a snippet like `…our flagship Level 1 class is held every Tuesday…` and immediately know it's the class name that needs to become "Intro to Neurofascial Training", not some incidental use of the word "level".

For the rare edge case — a page that mentions "Level 1" in a way that *shouldn't* be renamed, like historical text or a testimonial quoting someone — I can spot it in the context column and skip that row. That judgment call is exactly the part you don't want a blind database replacement making on your behalf.

## **Takeaway**

The temptation with a task like this is to just start clicking. It feels productive. But on a site of any real size, a half hour spent writing a scanner pays for itself almost immediately — not just in saved time, but in the confidence that you actually caught everything and didn't quietly corrupt adjacent content along the way.

Sometimes the most valuable thing automation gives you isn't speed. It's the audit trail.

## References

*   *New XML Sitemaps Functionality in WordPress 5.5 — Make WordPress Core:* [*https://make.wordpress.org/core/2020/07/22/new-xml-sitemaps-functionality-in-wordpress-5-5/*](https://make.wordpress.org/core/2020/07/22/new-xml-sitemaps-functionality-in-wordpress-5-5/)
    
*   *Sitemaps XML Protocol — sitemaps.org:* [*https://www.sitemaps.org/protocol.html*](https://www.sitemaps.org/protocol.html)
    
*   `body_class()` *Function Reference — WordPress Developer Resources:* [*https://developer.wordpress.org/reference/functions/body\_class/*](https://developer.wordpress.org/reference/functions/body_class/)
    
*   `get_body_class()` *Function Reference — WordPress Developer Resources:* [*https://developer.wordpress.org/reference/functions/get\_body\_class/*](https://developer.wordpress.org/reference/functions/get_body_class/)
    
*   *re — Regular expression operations — Python 3 docs:* [*https://docs.python.org/3/library/re.html*](https://docs.python.org/3/library/re.html)
    
*   *Better Search Replace — WordPress Plugin Directory:* [*https://wordpress.org/plugins/better-search-replace/*](https://wordpress.org/plugins/better-search-replace/)
    
*   *Requests: HTTP for Humans —* [*https://requests.readthedocs.io/*](https://requests.readthedocs.io/)
    
*   *Beautiful Soup Documentation — https://www.crummy.com/software/BeautifulSoup/bs4/doc/*