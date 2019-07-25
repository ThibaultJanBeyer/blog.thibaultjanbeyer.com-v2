---
layout: post
title: Easing the Cross-Origin pain in JavaScript
excerpt: I’ve been developing frontends all my life. Nevertheless, every time I experience a CORS issue, I feel deep pain. Today I’ll try to ease that pain for all of us!
image: /assets/cors/girl.jpg
image_alt: A picture of a ever repeating pattern
---

I’ve been developing front-end applications all my life. Nevertheless, every time I experience a CORS issue, I feel deep pain. Today I’ll try to ease that pain for all of us!

## TL;DR

<blockquote>
  <ul>
    <li>
      It is a front-end dev’s problem that can only be fixed by a backend dev  
    </li>
    <li>
      The Same Origin Policy is only enforced by the browsers (front-end)  
    </li>
    <li>
      CORS has to be enabled by on the server (back-end)  
    </li>
    <li>
      It is important that in the server response, the `Access-Control-Allow-Origin` header is set.  
    </li>
    <li>
      It is very important that in the browsers request headers the `Origin` header is set.  
    </li>
    <li>
      If you experience any issues with so called tainted content, check whether one of those headers is missing.  
    </li>
    <li>
      https://enable-cors.org/index.html is a website that explains how to enable CORS on most backend technologies.
    </li>
  </ul>
</blockquote>

## TOC

### Understanding

1 [What the f\* is CORS anyways?](#what-the-f-is-cors-anyways)

- 1.1. [Same Origin Policy (SOP)](#same-origin-policy-sop)
- 1.2. [How is the Same Origin Policy useful?](#how-is-the-same-origin-policy-useful)
- 1.3. [Examples of the Same Origin Policy](#examples-of-the-same-origin-policy)
- 1.4. [So should we keep enforcing the Same Origin Policy?](#so-should-we-keep-enforcing-the-same-origin-policy)

### Usage

2 [Cross-Origin Resource Sharing (CORS) to the rescue!](#cross-origin-resource-sharing-cors-to-the-rescue)

- 2.1. [When is CORS triggered?](#when-is-cors-triggered)
- 2.2. [What requests can use CORS?](#what-requests-can-use-cors)
- 2.3. [How to use CORS correctly?](#how-to-use-cors-correctly)
- 2.4. [What Headers do we need?](#what-headers-do-we-need)
- 2.5. [How to enable CORS on my Server?](#how-to-enable-cors-on-my-server)
- 2.6. [How to use resources like images, videos, links and scripts with CORS?](#how-to-use-resources-like-images-videos-links-and-scripts-with-cors)

### Avoiding

3 [How to bypass the Same Origin Policy (SOP) and avoid CORS?](#how-to-bypass-the-same-origin-policy-sop-and-avoid-cors)

- 3.1. [PostMessage Communication](#postmessage-communication)
- 3.2. [Use a proxy](#use-a-proxy)

### End

4 [Further Reading](#further-reading)  
5 [Final Words](#final-words)

## What the f\* is CORS anyways?

To understand Cross-Origin Resource Sharing (CORS) we have to understand the [Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy):

## Same Origin Policy (SOP)

The Same-Origin Policy is a web-browsers’ standard that prevents malicious external scripts to access sensitive data from one web-page.
Generally it only permits a web-page to load data that comes from the same origin.
For example if a web-page on the origin `www.foo.com/bar` is requesting an image from `www.foo.com/image.png` it will work by default since the resource is on the same origin as the request comes from.
But, when `www.foo.com` loads resources from `www.baz.com` they will either be tainted (meaning they can be displayed but not read from in JavaScript) or forbidden completely by the same origin policy since `baz.com` is not the same origin as `foo.com`.
An origin is defined as a combination of [URI scheme](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Examples), [domain](https://en.wikipedia.org/wiki/Hostname), and [port number](<https://en.wikipedia.org/wiki/Port_(computer_networking)>).

So, before CORS came into play, it was almost impossible to load resources from other web-pages. Now CORS is a mechanism that make this sharing possible. We’ll see how in a minute.

### How is the Same Origin Policy useful?

It’s a concept to isolate one website from another.
It is what stops for example Facebook accessing the data from your bank account.
<i class="hilite">This rule is enforced by the browser. If it’s not a browser, then the Same Origin Policy does not apply.</i>

### Examples of the Same Origin Policy

#### You could for example from website A open another website B and read/manipulate its content like so:

```javascript
// on www.hacker.com
const bank = window.open('https://www.yourbank.com/transactions', 'right');
console.log(bank.document.body); // same origin “error”
```

Here we are on the website hacker.com, this website opens a new browser tab for us which loads yourbank.com (suppose that’s the name of our bank).
Now suppose I’m logged in already, hacker.com could read all information of that page. Or worse, could send me to the banks official login page but capture my credentials on the way!

Luckily the Same Origin Policy prevents that. So the actual output will be an error similar to: `Error: Blocked a frame with origin "https://www.hacker.com/" from accessing a cross-origin frame`.
Which is a good example on how one website/window/origin is isolated from another by the Same Origin Policy.

#### The same could be done using frames:

```html
<!-- on www.hacker.com -->
<frameset cols="50%,50%">
  <frame name="framea" src="https://www.hacker.com/hack" />
  <frame name="frameb" src="https://www.yourbank.com/transactions" />
</frameset>
<script>
  console.log(window.framea.document.body); // works
  console.log(window.frameb.document.body); // same origin “error”
</script>
```

Here is some ancient example of frames. This is how websites used to be ~15 years ago.
Although the same principles apply on `iframes`, which you are probably more familiar with.
Same as with example one, because of the Same Origin Policy the website hacker.com can only access the contents of `framea` (since it is on the same origin) but not `frameb`!

#### Images, Video, CSS and JS:

Any image, video, CSS and JS code <i class="hilite">can be loaded and executed from any other site but not read</i>. The image will be shown, the video will be shown and the CSS will be applied. However, you won’t be able to read the contents with JavaScript:

```html
<!-- on www.hacker.com -->
<link rel="stylesheet" type="text/css" href="hacker.css" />
<link
  rel="stylesheet"
  type="text/css"
  href="https://www.yourbank.com/transactions.css"
/>
<script>
  console.log(document.styleSheets[0].rules); // works
  console.log(document.styleSheets[1].rules); // null (empty)
</script>
```

The same example applies to images, videos and JavaScript code. This is done to protect you in case there was any sensitive information in an image/video/css

#### HTTP Requests:

```javascript
// on www.hacker.com
fetch('https://hacker.com/info', { method: 'POST', body: 'hi' }); // works
fetch('https://yourbank.com/transactions', { method: 'POST', body: 'hi' }); // does not
```

Note that the request is actually happening: the browser sends the request to your bank, the browser will receive the response from your bank, but the browser will not let JavaScript access that response. You will get some kind of `No 'Access-Control-Allow-Origin' Headers present` error message instead.

As you can see, the Same Origin Policy is a set of rules implemented by the browsers that protect one web-page from others in the same browser. It isolated pages from one another.

### So should we keep enforcing the Same Origin Policy?

It depends. While it might be another layer of security, I can totally aggree with [ttwinlakkes](https://www.reddit.com/r/Frontend/comments/cfpf5a/what_are_your_painpoints_with_cors_cross_origin/euchfph/):
most of the times I have had problems with it is with protected APIs that already have protections in place. It might be a good idea in general, but it is a nuisance if your only cross-origin resources are protected public endpoints.

As you can see it really depends on your use case: Enabling Cross Origin Request for all domains **is not** a security risc per se and can make total sense. Generally, <i class="hilite">for 99% of public APIs the Same Origin Policy makes absolutely no sense</i>.

## Cross-Origin Resource Sharing (CORS) to the rescue!

With time came the conclusion that it is very often required to load resources from other origins.
For example, you might want to host your API endpoint on another origin than your web-app, i.e. `api.foo.com` vs `app.foo.com`.  
CORS is basically how you say “Do not apply the Same Origin Policy here”. I’ll explain exactly how in a bit.

### When is CORS triggered?

Whenever you make a HTTP Request to:

- a _different domain_ (i.e. foo.com calls baz.com)
- a _different subdomain_ (i.e. bar.foo.com calls bat.baz.com)
- a _different port_ (i.e. foo.com calls foo.com:9000)
- a _different protocol_ (i.e. https://foo.com calls http://foo.com).

### What requests can use CORS?

- Any `XMLHttpRequest` or `Fetch` requests.
- Web Fonts (as @font-face within CSS).
- WebGL textures.
- Images, Videos, Scripts and Links when the [crossorigin](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) attribute is set.

### How to use CORS correctly?

CORS allows a site to “opt-in” to weaken SOP and allow external pages to access and read data.

- Sender sends a request with an [origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Origin) header.
- Receiver answers with at least an [Access-Control-Allow-Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) header.

### What Headers do we need?

#### [Access-Control-Allow-Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)

Restricts which origins are allowed to make a request.
Use `*` to allow access from all origins or use a specific origin:

```
Access-Control-Allow-Origin: https://developer.mozilla.org
```

<i class="hilite">If you require the client to pass authentication headers (e.g. cookies) the value [can not be \*](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors/CORSNotSupportingCredentials) — it must be a fully qualified domain!</i>

#### [Access-Control-Allow-Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

Which [HTTP Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) are allowed to be made:

```
Access-Control-Allow-Methods: POST, GET, OPTIONS, PUT, PATCH, DELETE
```

#### [Access-Control-Allow-Credentials](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)

Only required if your server supports cookie auth.

```
Access-Control-Allow-Credentials: true
```

#### More access control headers:

These are less common, still, here is the list:

- [Access-Control-Allow-Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)
  A comma separated list of headers your server will allow to get. Skip this if you’re ok with all request headers.
- [Access-Control-Expose-Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Expose-Headers)
  A comma separated list indicating which headers can be exposed as part of the response.
- [Access-Control-Expose-Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Expose-Headers)
  Response header indicates which headers can be exposed as part of the response.
- [Access-Control-Max-Age](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Max-Age)
  Response header indicates how long the results of a preflight request can be cached.

For Requests:

- [Access-Control-Request-Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Request-Headers)
  Request header is used by browsers when issuing a preflight request, to let the server know which Headers will be used when the actual request is made.
- [Access-Control-Request-Method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Request-Method)
  Request header is used by browsers when issuing a preflight request, to let the server know which HTTP method will be used when the actual request is made.

### How to enable CORS on my Server?

The fabulous website [enable-cors.org](https://enable-cors.org/server.html) has a list on how to enable CORS for Apache, nginx, PHP, Tomcat, Caddy, Meteor and many more.
For NodeJS you can either write them yourself or use this nice [express middleware](https://expressjs.com/en/resources/middleware/cors.html).

```javascript
const express = require('express');
const app = express();

app.use((req, res, next) => {
  res.append('Access-Control-Allow-Origin', ['*']);
  res.append(
    'Access-Control-Allow-Methods',
    'GET,PUT,POST,DELETE,OPTIONS,PATCH'
  );
  next();
});

app.post('/', async (req, res) => {
  res.send('ok');
});

app.listen(3000, function() {
  console.log('Example app listening on port 3000!');
});
```

### How to use resources like images, videos, links and scripts with CORS?

In HTML, there is an attribute for that! It’s called [crossorigin](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) and here is how you use it:

```html
<img crossorigin="anonymous" src="https://foo.com/img" />
<!-- OR -->
<img crossorigin="use-credentials" src="https://foo.com/img" />
<!-- Also works with other resources: -->
<script
  crossorigin="anonymous"
  src="https://example.com/example-framework.js"
></script>
<link crossorigin="use-credentials" rel="manifest" href="/app.webmanifest" />
<video
  crossorigin="anonymous"
  src="https://example.com/videofile.ogg"
  autoplay
  poster="posterimage.jpg"
></video>
```

Note:

- If you load those resources without the crossorigin working, they are referred as `tainted`. I.e. a Cross Origin Image that was included without respecting CORS is referred as `Tainted Image` and its manipulation is restricted.
- The Server still has to have CORS enabled and send the image with an `Access-Control-Allow-Credentials` Header
- In some older browsers [and in React](https://github.com/facebook/react/issues/14035) it is very important that the `crossorigin` attribute is set before the `src` attribute otherwise it might be ignored!

## How to bypass the Same Origin Policy (SOP) and avoid CORS?

### PostMessage Communication

PostMessage allow sites to “opt-in” to remove SOP.

- Sender specifies where it sends to.
- Receiver checks which origin it received the message from.

#### Example

```javascript
// Sender (parentsite)
window.frameA.postMessage(message, 'http://siteA.com');
// Receiver (siteA)
window.addEventListener('message', event => {
  if (event.origin !== 'http://parentsite.com') {
    console.log('wrong origin');
  } else {
    document.body.innerText = event.data;
  }
});
```

However, the relationship has to match the origin of the receiver.
Read more: https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage

### Use a proxy

Since it is only the browser that enforces the SOP you can place a Server in the middle of your request:

- Sender sends the request a Server on the same origin or with CORS enabled
- The Server redirects the request to the external recipient
- The Server gets the response and passes it back to the Sender

Why does it work? Because as described above, SOP doesn’t apply server-to-server communication, only to browser-to-server.

#### Example

```javascript
var proxy = require('express-http-proxy');
var app = require('express')();

app.use('/proxy', proxy('www.google.com'));
```

This is a super basic example of a basic Proxy server in NodeJS with express using [express-http-proxy](https://www.npmjs.com/package/express-http-proxy).
It takes any request to the `/proxy` endpoint and passes it through to `www.google.com`, returning the response to the caller.

#### Other Proxies

- Webpack has its own proxy included: [https://webpack.js.org/configuration/dev-server/#devserverproxy](https://webpack.js.org/configuration/dev-server/#devserverproxy)
- Robwu has hosted a proxy for us: [https://cors-anywhere.herokuapp.com/](https://cors-anywhere.herokuapp.com/) | [https://github.com/Rob--W/cors-anywhere/](https://github.com/Rob--W/cors-anywhere/)

## Further Reading

If you would like tho know more about CORS and SOP, I can recommend following resources for a deep dive:

- [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) explained on MDN in detail.
- [Understanding CORS](https://medium.com/@baphemot/understanding-cors-18ad6b478e2b) gives you another brief explanation on the topic and the headers.
- The [Aura Security Research Division article on SOP](https://research.aurainfosec.io/same-origin-policy/) goes deeper into the browser behavior with Cross Origin Requests.
- [1h OWASP video about Same Origin](https://www.youtube.com/watch?v=zul8TtVS-64) OWASP is a great resource to learn about web security and in this video they explain the same origin policy in detail.

## Final words

Congratulations! You just learned all you have to know about CORS and SOP.

PS: don’t forget to share the knowledge and to follow me <a href="https://twitter.com/ThibaultBeyer">on twitter <i class="fab fa-twitter"></i></a> or via <a class="toggle-sidebar" data-a11y-dialog-show="subscribe-dialog" href="#site-sidebar">email <i class="fa fa-fw fa-columns"></i></a> if you liked this post :)
