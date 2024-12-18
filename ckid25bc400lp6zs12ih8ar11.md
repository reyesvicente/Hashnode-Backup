---
title: "6 most common a11y issues and how to solve them"
datePublished: Sun Dec 06 2020 11:43:51 GMT+0000 (Coordinated Universal Time)
cuid: ckid25bc400lp6zs12ih8ar11
slug: 6-most-common-a11y-issues-and-how-to-solve-them
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1609336930527/VSi6OCd_c.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1609336937693/TCCHN-L3-.jpeg
tags: accessibility

---

%%[voicera]
*<span>Photo by <a href="https://unsplash.com/@pgreen1983?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Paul Green</a> on <a href="https://unsplash.com/s/photos/accessibility?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>*

This article is for readers who already know about implementing accessibility and is looking for an article that can review them on the subject.

A few weeks ago, Nick - Senior Developer at DEV/Forem - tweeted about gifting a book about accessibility. He did not want followers. He just required the receivers to write an article about the book. 

Every year, WebAIM evaluates the accessibility of the top 1 million website homepages by running automated tests using their API. The results are surprisingly bad. These are the results from the February 2020 evaluation:

I. 86.3% had low contrast texts

- Color and contrast use is vital to accessibility.  Some visually disabled users must be able to perceive content on web pages. Thus, WCAG defined a minimum contrast level of 4.5:1 except for Large Texts, Incidental(text/images of text which are inactive in a UI component), and texts on logos for a webpage to be more user-friendly. [Read more](https://webaim.org/articles/contrast/#ratio).

II. 66% had a missing alternative text for images

- Adding alternative texts on images is one of the easiest to do but challenging to implement correctly. Alt-text is vital because it makes sure that our images are well-perceived by everyone. Alt-text also helps users understand the purpose of an image. Finally, alt-text also helps with SEO.

- Alternative text attributes should be accurate and aligned with the context of the image. It should be succinct and not redundant.

III. 59.9% had empty links

- Recently, upon checking the lighthouse score of a [side project](http://covid19ph-unofficial.icvn.tech/), I stumbled upon an error on the home page where google lighthouse pointed out links that had ambiguous texts such as "Read more". Check the commit [f38fae5](https://github.com/reyesvicente/unofficial-covidtracker-ph/commit/f38fae52b49f2df8c3ae0161ec3c5ff2b929226f?branch=f38fae52b49f2df8c3ae0161ec3c5ff2b929226f&diff=split#) on my GitHub to see it. It is better to be more descriptive on link texts rather than use "here", "more" or "continue" to lessen the hurt on your google lighthouse's score.

IV. 53.8% had missing form input labels

- A label describes the function of a form and should be seen adjacent to the input. Different users may or may not see the form label's connection, and it is critical to miss it on a website. One of the advantages of having form labels is that the form sets to focus when clicking the label. Here are [examples](https://webaim.org/techniques/forms/controls#input).

V. 28.7% had empty buttons

- Buttons do things. Buttons are call-to-actions on a site that work the same way as links but perform actions such as a form submission, opening a modal, or an accordion. When deciding over a button rather than a link, a good reminder is when a link has no other value other than "#" and an onClick event listener is on a link.

VI. 28% had missing document language

- Document language is necessary because you want users to know that they are reading a webpage in their appropriate language and dialect. Essentially, a "lang='en'" is added to the <html> tag to fix the issue. Easy peasy.

More issues have to be tackled in this topic. Furthermore, as a disclaimer, I'm continually learning about a11y and depend on google lighthouse to fix the a11y issues on websites I work on as a freelance developer.
