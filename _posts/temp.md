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

## How to add http-interceptor to your Angular project

## Intercepting outgoing requests and making your requests consistent

## Intercepting incoming responses and handling a `504 Gateway Timeout` response
