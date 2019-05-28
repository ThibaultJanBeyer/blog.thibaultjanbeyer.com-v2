---
layout: post
title: A Quick guide to JavaScript Patterns
excerpt: TL;DR ! Today, JavaScript Patterns. What is it? Explained with JavaScript examples and real life examples.
image: /assets/patterns/pattern.jpg
image_alt: A picture of a ever repeating pattern
---

Here, I will give you a brief explanation on each pattern I stuble across. I will find a real life reference for each pattern and explain it in such an easy way that is easy to remember. That give you a quick and good overview about complex topics. This page is perfect to bookmark and review in case you need to refresh your knowledge. It is also a living document, by that I mean that I will constantly update this article always explaining new patterns I find in the simplest possible way. I’ll also add resources in case you want to dig deeper. Without further ado, let’s have a look at the JavaScript Patterns. Enjoy reading!

## TOC

1. [Singleton Pattern](#singleton-pattern)
2. [Observer Pattern](#observer-pattern)
3. [Publish Subscribe Messaging Pattern](#publish-subscribe-messaging-pattern)

## Singleton Pattern

The singleton pattern is the simple concept of restricting the instantiation of a class to one object/instance.

### Real Life Example

Singletons are everywhere, basically everything that is accessed by multiple persons and only available once.
Let’s say that in your office there is only one printer that everyone can use. Then that printer might be called a singleton.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/---.png"
  alt="---"
/></p>

### JavaScript Example

- In redux the state could be called a singleton since there is only one state tree available.

A singleton could look something like this:

```javascript
class Singleton {
  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    // Initialize object
    return Singleton.instance;
  }
  // Properties & Methods
}

const instance = new Singleton();
export default instance;
```

_Please note that this is just an example. A singleton can be written in a variety of ways. How you write the singleton pattern is up to you. Singleton is just an abstract idea of only having instances once._

## Observer Pattern

The Observer Pattern is basically a part called _subject_ that maintains a list of so called _observers_ and notifies them of changes.

### Real Life Example

A newspaper subscription could be an observer pattern. People can be added to a list, when there is a new newspaper coming out they will go through the list and send out the newspaper to each of the observers/subscribers.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/---.png"
  alt="---"
/></p>

### JavaScript Example

- Talking about redux, the `connect` method is an example of an observer pattern.
- Angulars two ways binding is also an example of an observer pattern.

An observer pattern could be something like that:

```javascript
// @todo
```

## Publish Subscribe Messaging Pattern

@todo
