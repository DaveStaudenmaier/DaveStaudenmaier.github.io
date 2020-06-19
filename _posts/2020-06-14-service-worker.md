---
layout: post
title: How to use service worker for an Angular progressive web app to work offline
---

In this blog, I will show you how you can add Service Worker to an Angular application and then configure it to work offline, including chaching some remote data.   

I will also show you how to do testing in your development environment and how to detect new versions of the service worker and notify the user that an update is available. 

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/service-worker)

Angular Version: 9

----
****

## What is a service worker and what does it do?

If you are creating a progressive web app, the service worker is a critical part of your application.   The service worker is a script that runs in the background on a separate thread than your web app.  It will keep running whether your web app is running or not and now sits between your web-app and the server, acting as a proxy for requests (i.e. fetching assests of page or data from an API).  The service worker can listen to these requests and do things like caching the responses or returning these cached responses back to your page rather than fetching from the server.

The service worker also gives us advanced features such as push-notifications, but the feature we will be discussing here is offline data storage, allowing your PWA to function even if there user is not connected to the internet!

Some special notes:

- It is important to understand that service worker requires HTTPS in production, though you can test with localhost.  I will show you how to do this.
- All of the major browsers now support service worker!   Check out this [link](https://jakearchibald.github.io/isserviceworkerready/) for the latest information.
- If you are unfamiliar with the service worker life cycle, you will want to read this [article](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle) by Jake Archibald.

You will see in this blog how the Angular PWA package makes working with the service worker easy!

## How do I test my service worker changes during development?

If you want to test functionality specific to service worker such as offline capability, you cannot use `ng serve` as you normally would.  Instead you will need to use an http server.   I use `http-server`, a light-weight node server to launch a simple node-based server to host the content of the project's distribution files.   This means that you must use  `ng build --prod` to create the production distribution files that it will use.   

I will create a brand new Angular 9 project using `ng new` called *service-worker*, run `ng build --prod` and serve it using the http server `http-server -p 8080 -c-1 dist/service-worker` and then access it in the browser at `localhost:8080`.  

The initial Angular project works fine online:

<img src="/images/initial-no-sw.png">

But if you take it offline, using Chrome's developer tools, you get the downasaur!

<img src="/images/initial-no-sw-offline.png">

Let's add the service worker to take care of that.  

## How do I add service worker to my Angular application?

Enter `ng add @angular/pwa` in your project directory on the command line to add PWA support for your application, which includes the service worker and web manifest.  

Then run `ng build --prod` again and go back to `localhost:8080`.  You will need to close the tab or refresh the page to get the service worker activated.   Now if you run the project offline, it still works!

<img src="/images/with-sw-offline.png">
          
In Chrome Developer tools, you can see the service worker running:

<img src="/images/with-sw.png">

## What do I get out of the box?

Let's see how `@angular/pwa` changed our application.

**index.html**

Added `<noscript>Please enable JavaScript to continue using this application.</noscript>` under the `<app-root>` selector as this project cannot work without JavaScript enabled.

Added `<link rel="manifest" href="manifest.webmanifest">` which points to the new web manifest file `manifest.webmanifest`, which allows you to save your app with an icon on the phone among other things.

Added `<meta name="theme-color" content="#1976d2">` metadata for theme.

**app.module.ts**

Imports the service-worker module from `@angular/service-worker` and adds this under the imports array: `ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production })`, which registers a service worker file called `ngsw-worker.js` which is enabled only for production.    You won't see `ngsw-worker.js` as it is generated when you build for production but you can find it in the disribution folder.  

**assets**

Added icons for the manifest file.

![icons](/images/icons.png)

**angular.json**

Added a service-worker configuration pointing to a new file called `ngsw-config.json`:

![sw-config](/images/sw-config.png)

Here is the contents of the `ngsw-config.json` file:

![config-json](/images/sw-config-json.png)

We will discuss this file below to understand what is going on, but first, let's see what is happening behind the scenes.

## What is happening behind the scenes when offline?

Using the Chrome developer tools, we can see on the Network tab that the service worker is now serving the content:

![sw-network](/images/offline-network.png)

## What is happening in the `ngsw-config.json` file?

We saw above that taking the app offline, already cached some resources so that it worked offline.   But how did it know what to cache?  Let's look at the `ngsw-config.json` file again.

![config-json](/images/sw-config-json.png)

`"index": "/index.html"` tells the service worker which page is the root page of our application.

The assetGroups array, `"assetGroups":`, tells the service worker which **static** assets it should cache.   The first set named **app** tells service-worker to cache our favicon, index.html, the web manifest file, all styles in the root folder and all JavaScript files in the root folder.  You can change these to, for instance, load all CSS in any CSS folder. InstallMode of **prefetch** tells service-worker to cache these assets **before** loading the app.  

The **assets** assetGroup is configured a little differently.   InstallMode of **lazy** tells service worker to cache these assets only as they are used.   This means if the user loses their internet connection before these assets are loaded the first time, the user will not see them.  It's a good idea, however, to load some assets on an as-needed-basis to speed up initial load of the app.   updateMode of **prefetch** tells service-worker that when a new version of the app is available and the user is already using the app, it can prefetch these assets or lazy load them.   In this section all files under **assets** folder and sub-folders are loaded as well as all files with certain file extensions such as svg, jpg, png, etc.  

You can see how you can use these assetGroups to prefetch or lazy load any assets you desire.   Just be careful not to load too much on prefetch or it will affect your apps performance.

## What about dynamic content?

I am going to change our little app and add some dynamic content so we can learn how to cache that as well.  

First I will add static content, replacing the html in `app.component.html`, displaying a title and an image I put in the assets/images folder: 

```typescript
<h1 class="heading">My PWA</h1>
<img src="./../assets/images/redwoods.png" class="image">
<br>
```

Now, build the app and run it using http-server.   You should close the tab in the browser you were using and go to localhost:8080 again.  And this is what we see:

![pwa initial](/images/my-pwa1.png)

And if we take it offline, it looks the same.  as the static assets were all loaded and cached. 

Now let's add some dynamic content.  In `app.component.ts` I added this method:

```typescript
  private initializeData() {
    this.http.get<Post[]>('https://jsonplaceholder.typicode.com/posts')
    .subscribe(dummyData => {
      this.posts = dummyData;
    });
  }
```

The initializeData method calls a free dummy data API that we can use to get some 'posts' data and store in an array which we display in `app.component.html`:

```html
<div *ngFor="let post of posts">
  <span>{{ post.title }}</span>
</div>
```

Let's build our code and see how it looks.  Refresh localhost:8080 to see the new dynamic data displayed: 

![with dynamic data](/images/my-pwa2.png)

But when we take it offline the data does not display. 

![dynamid data offline](/images/my-pwa3.png)

For our purposes, let's assume that this data is very important and we want to load it dynamically so it will display offline.  

## How can I cache dynamic data?

Let's go back to `ngsw-config.json` and add a new **dataGroups** array after the **assetGroups** array like this:

```typescript
  "dataGroups": [
    {
      "name": "data",
      "urls": [
        "https://jsonplaceholder.typicode.com/posts"
      ],
      "cacheConfig": {
        "maxSize": 5,
        "maxAge": "6h",
        "timeout": "5s",
        "strategy": "freshness"
      }
    }
  ]
```

I gave our dataGroup the name **data**, but you can choose what ever name you want.   **assetGroups** is for static assets and **dataGroups** is for dynamic data.   

I added the URL for the API we are using in the **urls** array and then **cacheConfig** which configures how this data should be cached.

**maxSize** defines how may responses we wish to cache.  This is not the number of records, but the number of responses.   Be careful. We don't want to cache everything as our space is limited by the browser. 

**maxAge** defines how old the data in the cache should be before the service-worker fetches new data.  It should be in hours (6h), days (6d), minutes (6m), etc.

**timeout** defines how long the service-worker should wait before using the cache.  Maybe you want it to try to fetch from the server, but if it takes longer than 5 seconds, get it from the cache. 

**strategy** has two types.  
- **freshness**, which tells service-worker to always try to fetch from the server first and only fetch from the cache if offline, taking into account **timeout**
- **performance**, which tries to get something on the screen as quickly as possible, taking into account the **maxAge**.

Now, after we build again, we see that dynamic data is handled offline. 

![dynamid data offline](/images/my-pwa4.png)


## How can I detect a new version of service worker is available and notify the user?
