---
layout: post
title: Different ways to pass data between components in Angular
---

In this blog, I will show you three ways to pass data between components in Angular using a real-world example.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/AngularShareData)

Angular Version: 9.1

----
****

## Introduction

To use Angular effectively it is critical to understand the many ways to pass data between components.   In this blog, I will discuss three of them.
- Parent to child using @Input
- Using Query Parameters to pass data between unrelated components
- Using a service to pass data between unrelated components

The example application in [GitHub](https://github.com/DaveStaudenmaier/AngularShareData) is a fictional national parks web app where you can filter the national parks list by state.  The home page has a search filter and a hook to a child component that shows the national parks.  Clicking on a national park will bring you to the detail about the park.  

So, we need to pass the search filter from the parent component to the child component and then pass the selected national park to the details component.

## Parent to child using @Input
Using @Input is the easiest way to pass data between from a parent component to a child component.  In the example from GitHub, the parent component is a search filter applied to the data shown below from a child component.   So, we need to pass the search text to the child component. 

<img src="/images/blog/share-data/search.png" height="500px">

We use property binding to bind the value of the search input box in the child component to the search property in the child component, where we can use it to filter our array of national parks.

<img src="/images/blog/share-data/Input.png" height="500px">

You must import Input in the child component

``` typescript
import { Input } from '@angular/core';
```

You use it like this

``` typescript
@Input() search: string;
```

An alias may be used if necessary

``` typescript
@Input('mySearch') search: string;
```

Check out the app in GitHub to see how you can use this technique to filter everytime the user enters a character, even though the details are in another component.

## Unrelated component using Query Parameters

When the component you want to pass data to is not a child component, one way you can pass data is through Query Parameters on the URL.  

*** Warning *** Do not pass sensitive data in the query string!  It is not secure.

In our app, we can use this technique to pass the selected national park to the parks-detail component.  As this is not sensitive information, it would be acceptable to use query parameters.  

<img src="/images/blog/share-data/query-params.png" height="500px">

In the calling component, we pass an additional parameter when routing using the navigate method of Router.   

*** Note*** While officially, there is no limit specified by RFC 2616, many security protocols and recommendations state that maxQueryStrings should be limited to 1024 characters, while the entire URL, including the querystring, should not exceed 2048 characters.  

In the called component, we subscribe to queryParams on the ActivatedRoute in OnInit. 

## Unrelated component using a service

A more secure way than query parameters to pass data between unrelated components is to use a service.   Here is the service I am using in our app

```typescript
import { Injectable } from '@angular/core';

export interface INationalParks {
  name: string;
  state: string;
  established: string;
  area: string;
  visitors: string;
}


export interface IshareData {
  parks: INationalParks;
}

@Injectable({
  providedIn: 'root'
})
export class DataShareService {
  private data: IshareData;

  constructor() {
    this.initializeData();
  }

  getData(type: string): any {
    return this.data[type];
  }

  setData(type: string, data: any) {
    this.data[type] = data;
  }

  initializeData() {
    this.data = {
      parks: {
        name: null,
        state: null,
        established: null,
        area: null,
        visitors: null
      }
    };
  }
}

```

The calling component uses the setData method of the share-data service to store the data.   The called component uses the getData method to get the data.  

<img src="/images/blog/share-data/share-data-svc.png" height="500px">

You can set up other data types using this method as well.  For example:

```typescript
export interface INationalParks {
  name: string;
  state: string;
  established: string;
  area: string;
  visitors: string;
}

export interface Iwaterfalls {
  waterfallName: string;
  waterfallPark: string;
}

export interface IshareData {
  parks: INationalParks;
  waterfalls: Iwaterfalls
}
```

## Conclusion

We looked at three different ways to pass data between components in Angular including:
- Parent to child using @Input
- Using Query Parameters to pass data between unrelated components
- Using a service to pass data between unrelated components

I wanted to focus on these three, but you can also pass data from child to parent using @Output and call methods and pass data to a child using @ViewChild.

Also, for a large application, using a service can get cumbersome.  This is when you want to check out Angular's NGRX state management system, which I will save for another blog.

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments!

Keep on developing!
