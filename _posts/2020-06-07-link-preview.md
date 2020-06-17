---
layout: post
title: How to add link preview to your Angular application, using linkpreview.net
---

In this blog, I will show you how you can add a link preview to any application, such as a forum, where you want the user to be able to add links and see the link preview.

Angular Version: 9

<img src="/images/link-preview.png" height="500px">

----
****

**What is a Link Preview?**

You've seen how Facebook and other social media platforms display a link in a post.  It comes with a pretty picture, the url, a title and a description, like this: 

![Facebook Example](/images/link-preview-fb.png)

What makes this work is metadata called the [Open Graph Protocol](https://ogp.me/#:~:text=Introduction,any%20other%20object%20on%20Facebook.).  Facebook launched the Open Graph protocol in 2010, and it is is now managed by the Open Web Foundation.  The Open Graph Protocol allows web sites to control what is displayed when a web site is shared.  

Here is the metadata for my own site, RVLikeMe.com, as an example:

```html
  <meta property="og:url" content="https://rvlikeme.com" />
  <meta property="og:site_name" content="RV Like Me" />
  <meta property="og:type" content="website" />
  <meta property="og:title" content="RV Like Me" />
  <meta property="og:description" content="Whether you are a full-time, part-time, wanna-be, stationary or mobile RVer, RV Like Me connects you with people living the RV lifestyle like you do." />
  <meta property="og:image" content="https://rvlikeme.com/assets/images/rvlikeme.jpg" />
```

**How to you get this metadata from a web site?**

One way to get it is to create an account on [linkpreview.net](https://linkpreview.net).  Once registered you will get a key that you can use in an API call, passing a URL and receiving back a JSON object with the metadata from that web site.  

Once you have the JSON object from linkpreview.net, it's just a matter of formatting it for display.  You could show it when the user hovers over a link or show it as part of a post.   In our example here, we will use it to show the user the preview before we store the link for display.   I will show you the pertinent parts and you can see my GitHub for the full example.

**Connect with linkpreview.net**

After you get your key from [linkpreview.net](https://linkpreview.net), (you would replace `<key>` below with your key), using an Observable to subscribe, you can store the object for display.  
```typescript
  export interface IlinkPreview {
    description: string;
    image: string;
    title: string;
    url: string;
  }
  
  ...
  
  preview: IlinkPreview;
  
  ...
  
  getLinkPreview(link: string): Observable<any> {
    const api = 'https://api.linkpreview.net/?key=<key>&q=' + link;
    return this.http.get(api);
  }
  
  onPreview() {
    this.getLinkPreview(this.link.value)
    .subscribe(preview => {
      this.preview = preview;

      if (!this.preview.title) {
        this.preview.title = this.preview.url;
      }

    }, error => {
      this.preview.url = this.link.value;
      this.preview.title = this.preview.url;
    });
  }
```

If the URL is invalid, you will reveive an error code.  You can catch these codes to do different things, but here I am just storing the URL the user entered in the title so that something is displayed whether we have a preview or not.  

----
****

For my simple example, I did not do much styling.   Here is the HTML and styles I applied:

```html
  <div *ngIf="preview.url" class="preview">
    <div [hidden]="!preview.image">
      <img class="image" [src]="preview.image">
    </div>
    <div class="preview-text">
      <div [hidden]="!preview.url" class="url">
        <span>{{preview.url}}</span>
      </div>
      <div [hidden]="!preview.title" class="title">
        <span>{{preview.title}}</span>
      </div>
      <div [hidden]="!preview.description" class="description">
        <span>{{preview.description}}</span>
      </div>
    </div>
  </div>
```

```css
.preview {
  width: 350px;
  background-color:#bdbdbd;
  border-style: solid;
  border-color: grey;
  border-width: 1px;
}

.preview-text {
  padding: 10px;
}
```

**And here is what the user sees**

<img src="/images/link-preview.png" height="500px">

One important note.  If your application is secured with https, you will not be able to link to a non-https web site for security reasons. 

You can find the complete code on my [GitHub](https://github.com/DaveStaudenmaier/LinkPreview).

Keep on Developing!
