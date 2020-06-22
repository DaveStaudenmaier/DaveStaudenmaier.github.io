---
layout: post
title: Add an http-interceptor service in Angular to modify outgoing requests and handle incoming responses
---

In this blog, I will show you how you can add an http-interceptor service to your Angular project to intercept outgoing requests to simplify and make your http requests consistent as well as intercept incoming responses to handle a `504 Gateway Timeout` situation by retrying automatically.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/http-interceptor)

Angular Version: 9

----
****

## What is an http-interceptor?

The job of an Angular's HttpInterceptor is to intercept and handle an HttpRequest or an HttpResponse, allowing you to add code to do something. Some common use-cases are:
- Add a token or some custom HTTP header information prior to sending a request to the server
- Catch an HTTP response to for data transformation
- Log all HTTP activity
- Detect a specific HTTP error response and take special action

In this blog we will intercept all HTTP requests in our Angular project and add a token and server location information before passing through to the server.   We will also interept HTTP responses to detect a `504 Gateway Timeout` and retry 3 times, with a delay between each attempt before passing the 504 on.

## Create new Angular Project

First, we will create an Angular project and add a simple HTTP service.   Use `ng new` to create a project:

*`ng new http-interceptor`*

Go to the project directory and add a service:

*`ng g s data`*

Now, let's add HTTP capability to our project.  In `app.module.ts` import the `HttpClientModule` and add it to the imports array.

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Then go to the new service, `data.service.ts` and add HttpClient

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class DataService {

  constructor(private http: HttpClient) { }
  
}
```

Now we will add a call to a dummy service that simply sends back the response code we select after a delay we configure.  Add this method in `data.service.ts':

```typescript
  getData(responseCode: number) {
    return this.http.get('http://httpstat.us/' + responseCode + '?sleep=1000');
  }
```

OK.  We have added HTTP capability to our project and an http get request method.   

Now, let's add a little code in `app.component.ts' to call the data service:

```typescript
import { Component } from '@angular/core';

import { DataService } from './data.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  showResult = false;
  httpResult: string;

  constructor(private data: DataService) {}

  onResponse(responseCode: number) {
    this.showResult = true;
    this.httpResult = 'Loading...';

    this.data.getData(responseCode)
    .subscribe(result => {
      this.httpResult = 'Success!';
    }, error => {
      this.httpResult  = 'Error! Status =' + error.code;
    });
  }
}
```

Here, we imported we imported our data service and created a method to call the getData method in the data service.

Great!  Now all we need to do is add a little HTML to `app.component.html`, replacing the default HTML with this:

```html
<h1>Http Interceptor</h1>

<button (click)="onResponse(200)" class="button">Response 200</button>
<br>
<br>
<p *ngIf="showResult">Result = {{ httpResult }}</p>
```

When you run the app, you can make a call and get a 'successful' response.   

![screen shot](/images/screen-shot1.png)

Now we can add the http-interceptor service.

## How to add http-interceptor to your Angular project

Add a new service to our Angular project:

*`ng g s http-interceptor`*

In `http-interceptor.service.ts`, import the Http services we need from `@angular/common/http`.   Then add the HttpInterceptor interface to our class. 

```typescript
import { Injectable } from '@angular/core';
import {  HttpEvent,
          HttpInterceptor,
          HttpHandler,
          HttpRequest } from '@angular/common/http';

import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})

export class HttpInterceptorService implements HttpInterceptor {
  intercept(request: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    console.log('request=', request.url);
    return next.handle(request);
  }
}
```

All we will do for now is log the request url and pass the request through using `return next.handle(request)`. 

Next we must tell Agular to use our interceptor service in `app.module.ts':

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';

import { AppComponent } from './app.component';
import { HttpInterceptorService } from './http-interceptor.service';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: HttpInterceptorService
    },
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

We imported HTTP_INTERCEPTORS from `@angular/common/http` and instructed Angular to user our http interceptor service in the providers array.

## Intercepting outgoing requests and making your requests consistent

## Intercepting incoming responses and handling a `504 Gateway Timeout` response
