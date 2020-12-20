---
layout: post
title: Pre-process long-running API calls and publish via a BehaviorSubject in Angular
---

In this blog, I will go over how you can get API data in advance so that when the user gets to the page that needs the data, it will be there.
This is useful when you have a potentially long-running API call and do not want to start the retrieval when the user arrives at the page
because they will have to wait.  Getting the data in advance will make the user experience more pleasant.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/preprocess-API-calls)

Angular Version: 10.1.0

----
****
## Overview

In AppComponent, subscribe to a service that makes the API calls. 

When data is returned, publish it via a BehaviorSubject.

In the secondary page that will require the data, subscribe to the BehaviorSubject and display the available data for the user.

## What will the user see?

While the user is reading the home page, the app will begin getting data behind the scenes.

<img src="/images/blog/preprocess-API-calls/home.png" height="400px">

There are two API calls initiated upon app startup.  The first takes 3 seconds and the second takes 6 seconds.   If you went to the view page before 3 seconds, you would see the deafult which is 'No Data'.

<img src="/images/blog/preprocess-API-calls/view-default.png" height="400px">

If you sent to the View page between 3 and 6 seconds you would see some of the data.

<img src="/images/blog/preprocess-API-calls/view-interim.png" height="400px">

If you went to the view page after 6 seconds, you will see all of the data.

<img src="/images/blog/preprocess-API-calls/view-done.png" height="400px">


## What is a BehaviorSubject?

A BehaviorSubject is a type of RxJs subject, which is a special type of observable that allows you to subscribe to messages like any other observable.  The unique feature of BehaviorSubjects are that it will always provide a value when subscribed to.  Thus, a default value must be provided so that any subscriptions that happen before data is assigned via next(), will receive the default data and the real data will arrive when available. 

## Why use a BehaviorSubject for pre-processing data?

If you have a long-running API call that you subscribe to when your page loads, the user will have to wait and watch a spinner spin until the data arrives.  You can pre-process the data by subscribing to the API call when the application first starts, perhaps in AppComponent.  

But then the data resides in AppComponent.  How do you get the data to the page that needs it?  

This is where the BehaviorSubject comes in.   As the data arrives from the API call, AppComponent can publish it to the BehaviorSubject, which will override the default values.   The page that requires the data can subscribe to the BehaviorSubject in ngOnInit so that it can populate the properties needed by the HTML view.  If there are things the user will be doing between AppComponent and the component that needs the data, such as filling in a form, viewing a dashboard, etc. then during that time the data will be retrieved so that it is available to the target page via the BehaviorSubect.

## Set up the BehaviorSubject

In the example application, in `data-service.service.ts` I have created these interfaces to represent data coming from two sources and combined into one data set.

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

First I defined and initialized a variable that represents the combined data of type interface StoredData.   Then I created the BehaviorSubject and set the initialized StoredData as the default.   Later, I will call setData, when real data is available, to populate the specific type of data recieved and then call next() to publish it.

``` typescript
private storedData: StoredData = this.initializeData();
private data = new BehaviorSubject<StoredData>(this.storedData);
data$ = this.data.asObservable();

setData(type: string, value: any): void {
  this.storedData[type] = value;
  this.data.next(this.storedData);
}

private initializeData(): StoredData {
  const data: StoredData = {
    data1: {
      dataValue1: 'No Data',
      dataValue2: ''
    },
    data2: {
      dataValue1: '',
      dataValue2: ''
    }
  };
  return data;
}
```

## The API calls

Typically API calls are https calls to a server.  In this case, to simplify the code, I have simulated long running API calls using the RxJs `delay` function.  I also used the setTimeOut to avoid populating the data until a millisecond before the delay is expired.

``` typescript
getData1(): Observable<DataStore1> {
  const self = this;
  setTimeout(function () {
    self.storedData.data1.dataValue1 = 'We';
    self.storedData.data1.dataValue2 = 'received';
  }, 2999);

  return of(this.storedData.data1)
  .pipe (delay(3000));
}

getData2(): Observable<DataStore2> {
  const self = this;
  setTimeout(function () {
    self.storedData.data2.dataValue1 = 'all';
    self.storedData.data2.dataValue2 = 'data';
  }, 5999);

  return of(this.storedData.data2)
  .pipe (delay(6000));
}
```

## Pre-processing in AppComponent

In `AppComponent` I subscribed to the API calls.   When the result is returned, I published to the BehaviorSubject via the setData method.

``` typescript
constructor(private dataService: DataService) {

  this.dataService.getData1()
  .subscribe(result => {
    this.dataService.setData('data1', result);
  });

  this.dataService.getData2()
  .subscribe(result => {
    this.dataService.setData('data2', result);
  });
}
```

## The Component Views

I created two components.  `Home` is the initial page.  `View` is the page that displays the data from the long-running API calls. 

In `ViewComponent` in `ngOnInit`, I subscribed to the behavior Subject and stored the latest data in a property for the HTML view.

``` typescript
import { DataService, StoredData } from './../data-service.service';

public returnedData: StoredData;

constructor(private dataService: DataService) { }
            
ngOnInit(): void {
  this.dataService.data$
  .subscribe(result => {
    this.returnedData = result;
  })
}
```
## Conclusion

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments!

Keep on developing!
