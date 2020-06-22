---
layout: post
title: Add an http-interceptor service in Angular to modify outgoing requests and handle incoming responses
---

In this blog, I will show you how you can add an http-interceptor service to your Angular project to intercept outgoing requests to simplify and make your http requests consistent as well as intercept incoming responses to handle a `504 Gateway Timeout` situation by retrying automatically.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/http-interceptor)

Angular Version: 9

----
****

## What is a service worker and what does it do?
