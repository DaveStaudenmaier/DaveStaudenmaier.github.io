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

The good news is the Angular PWA package makes working with the service worker easy!

**How do I add service worker to my Angular application?**

Enter `ng add @angular/pwa` in your project directory on the command line to add PWA support for your application, which includes the service worker and web manifest.  

**How do I test my service worker changes during development?**

If you want to test functionality specific to service worker such as offline capability, you cannot use `ng serve`.  Instead you will need to use a server.   I use http-server, a light-weight node server to launch a simple node-based server to host the content of the folder your Angular DIST folder.   This means that you must use  `ng build --prod` to create the production distribution files that it will use.   

After you build your project, run `http-server -p 8080 -c-1 dist/YourFolderName`.  Then you can access your application at `localhost:8080`.  

The initial Angular project works fine online but if you take it offline, the downasaur!

<img src="/images/initial-no-sw.png" width:"200px">

**What do I get out of the box?**

**How does service worker handle updates?**

**How can I cache data?**

**How can I detect a new version of service worker is available and notify the user?**
