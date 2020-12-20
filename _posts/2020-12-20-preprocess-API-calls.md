---
layout: post
title: Pre-process long-running API calls and publish via a BehaviorSubject in Angular
---

In this blog, I will go over how you can get API data in advance so that when the user gets to the page that needs the data, it will be there.
This is useful when you have a potentially long-running API call and do not want to start the retrieval when the user arrives at the page
because they will have to wait.  Getting the data in advance and publishing via a BehaviorSubject will make the user experience more pleasant.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/preprocess-API-calls)

Angular Version: 10.1.0

----
****
## Overview

In AppComponent, subscribe to a service that makes the API calls. 

When data is returned, publish it via a BehaviorSubject.

In the secondary page that will require the data, subscribe to the BehaviorSubject and display the available data for the user.

## What is a BehaviorSubject?

A BehaviorSubject is a type of RxJs subject, which is a special type of observable that allows you to subscribe to messages like any other observable.  The unique feature of BehaviorSubjects are that it will always provide a value when subscribed to.  Thus, a default value must be provided so that any subscriptions that happen before data is assigned via next(), will receive the default data and the real data will arrive when available. 

## Why use a BehaviorSubject for pre-processing data?

If you have a long-running API call that you subscribe to when your page loads, the user will have to wait and watch a spinner spin until the data arrives.  You can pre-process the data by subscribing to the API call when the application first starts, perhaps in AppComponent.  But then the data resides in AppComponent.  How do you get the data to the page that needs it?  This is where the BehaviorSubject comes in.   As the data arrives from the API call, AppComponent can publish it to the BehaviorSubject, which will override the default values.   The page that requires the data can subscribe to the BehaviorSubject in ngOnInit so that it can populate the properties needed by the HTML view.  If there are things the user will be doing between AppComponent and the component that needs the data such as filling in a form, viewing a dashboard, etc. then during that time the data will be retrieved so that it is available to the target page via the BehaviorSubect.

## Set up the BehaviorSubject

In the example application, go to ""data-service.service.ts"". 

I have created these interfaces to represent data coming from two sources and combined into one data set
``` typescript
export interface DataStore1 {
  dataValue1: string;
  dataValue2: string;
}

// Data format for second call
export interface DataStore2 {
  dataValue1: string;
  dataValue2: string;
}

// Combined data for publishing
export interface StoredData {
  data1: DataStore1;
  data2: DataStore2;
}
```

## Conclusion
<img src="/images/blog/share-data/search.png" height="500px">


Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments!

Keep on developing!
