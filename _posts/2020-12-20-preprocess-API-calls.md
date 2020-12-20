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

When data is returned, publish it via a BehaviorSubject

In the secondary page that will require the data, subscribe to the BehaviorSubject and display the available data for the user


## Conclusion
<img src="/images/blog/share-data/search.png" height="500px">


Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments!

Keep on developing!
