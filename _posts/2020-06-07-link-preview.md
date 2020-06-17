---
layout: post
title: How to add link preview to your Angular application, using linkpreview.net
---

In this blog, I will show you how you can add a link preview to any application, such as a forum, where you want the user to be able to add links and see the link preview.

Angular Version: 9

<img src="/images/link-preview.png" height="500px">

You can find the complete code on my GitHub at [https://github.com/DaveStaudenmaier/LinkPreview](https://github.com/DaveStaudenmaier/LinkPreview).

----
****

**What is a Link Preview?**

You've seen how Facebook and other social media platforms display a link in a post.  It comes with a pretty picture, the url a title and a description, like this: 

![Facebook Example](/images/link-preview-fb.png)

What makes this work is metadata called the [Open Graph Protocol](https://ogp.me/#:~:text=Introduction,any%20other%20object%20on%20Facebook.).  Facebook launched Open Graph protocol in 2010, and it is is now managed by the Open Web Foundation.  The Open Graph Protocol allows web sites to control what is displayed when a web site is shared.  

Here is the metadata for my own site, RVLikeMe.com, as an example:

```html
  <meta property="og:url" content="https://rvlikeme.com" />
  <meta property="og:site_name" content="RV Like Me" />
  <meta property="og:type" content="website" />
  <meta property="og:title" content="RV Like Me" />
  <meta property="og:description" content="Whether you are a full-time, part-time, wanna-be, stationary or mobile RVer, RV Like Me connects you with people living the RV lifestyle like you do." />
```
