---
layout: post
title: How to use service worker for an Angular progressive web app to work offline
---

In this blog, I will show you how you can add Service Worker to an Angular application and then configure it to work offline, including chaching some remote data.   

I will also show you how to do testing in your development environment and how to detect new versions of the service worker and notify the user that an update is available. 

Angular Version: 9

----
****

**What is a service worker and what does it do?**

If you are creating a progressive web app, the service worker is a critical part of your application.   The service worker is a script that runs in the background on a separate thread than your web app allowing for background sync and push notifications.  The service worker feature we will be discussing here is offline data storage, allowing your PWA to function even if there user is not connected to the internet!

It's important to understand that service worker requires HTTPS in production, though you can test with localhost.  I will show you how to do this in a little bit. 

All of the major browsers now support service worker!   Check out this [link](https://jakearchibald.github.io/isserviceworkerready/) for the latest information.

If you are unfamiliar with the service worker life cycle, you will want to read this [article](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle) by Jake Archibald.

**How do I add wervice worker to my Angular application?**

**How do I test my service worker changes during development?**

**What do I get out of the box?**

**How does service worker handle updates?**

**How can I cache data?**

**How can I detect a new version of service worker is available and notify the user?**
