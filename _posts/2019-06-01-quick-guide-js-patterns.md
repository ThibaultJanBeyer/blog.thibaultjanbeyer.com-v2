---
layout: post
title: A Quick guide to JavaScript Patterns
excerpt: TL;DR ! Today, JavaScript Design Patterns. What is it? Explained with JavaScript examples, real life examples and considering pros and cons.
image: /assets/patterns/pattern.jpg
image_alt: A picture of a ever repeating pattern
---

Here, I will give you a brief explanation on patterns I stumble across. I will find a real life reference for each pattern and explain it in such an easy way that is easy to remember. That give you a quick and good overview about complex topics. This page is perfect to bookmark and review in case you need to refresh your knowledge. It is also a living document, by that I mean that I will constantly update this article always explaining new patterns I find in the simplest possible way. I’ll also add resources in case you want to dig deeper. Without further ado, let’s have a look at the JavaScript Patterns. Enjoy reading!

## TOC

### Creational Patterns:

1. [Singleton Pattern](#singleton-pattern)
2. [Prototype Pattern](#prototype-pattern)
3. [Module Design Pattern](#module-design-pattern)

### Structural Patterns:

4. [Decorator Pattern](#decorator-pattern)
5. [Adapter Pattern](#adapter-pattern)

### Behavioral Patterns

6. [Observer Pattern](#observer-pattern)
7. [Publish Subscribe Messaging Pattern](#publish-subscribe-messaging-pattern)
8. [Mediator Pattern](#mediator-pattern)

### End

9. [Further Reading](#further-reading)
10. [Final Words](#final-words)

_Please note that all JS code examples are just examples. They can be written in a variety of ways. How you write the pattern is up to you. A Pattern is just an abstract idea._

## Singleton Pattern

The singleton pattern is the simple concept of restricting the instantiating of a class to one object/instance.

### Real Life Example

Singletons are everywhere, basically everything that is accessed by multiple persons and only available once.
Let’s say that in your office there is only one printer that everyone can use. Then that printer might be called a singleton.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/printer.jpg"
  alt="Huge letterpress printing machine"
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

_Please note that this and all further JS code examples are just examples. They can be written in a variety of ways. How you write the pattern is up to you. A Pattern is just an abstract idea._

### Pros and Cons of the Singleton Pattern

#### Pros

- Instance control: Singleton prevents other objects from instantiating their own copies of the Singleton object, ensuring that all objects access the single instance.
- Flexibility: Since the class controls the instantiating process, the class has the flexibility to change the instantiating process.
- Easy to implement: Easy to create, and we could use it anywhere for the lifetime of the app The advantage of Singleton over global variables is that you are absolutely sure of the number of instances when you use Singleton.

#### Cons

- They are a global mutable shared state. Their state is automatically shared across the entire app, and bugs can often start occurring when that state changes unexpectedly.
- The relationships between Singletons and the code that depends on them is usually not very well-defined. Since Singletons are so convenient and easy to access — using them extensively usually leads to very hard to maintain “spaghetti code” that doesn’t have clear separations between objects.
- Managing their life-cycle can be tricky. Since Singletons are alive during the entire lifespan of an application, managing them can be really hard. This also makes code that relies on Singletons really hard to test, since you can’t easily start from a “clean slate” in each test case.

## Prototype Pattern

It’s a pattern where we bind variables and methods to our object structure which can be initiated _N_ times. The object to be created is defined by a prototype instance (blueprint to be replicated or learned from) which is cloned to produce new objects.

### Real Life Example

It’s quite difficult to find a good real life example for such a pattern. Maybe the blueprint for a construction work could be called a prototype pattern.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/blueprint.jpg"
  alt="a blueprint for construction work"
/></p>

### JavaScript Example

- Prototypical example:

```javascript
// blueprint
function MyClass(bar) {
  this.foo = bar;
}

MyClass.prototype.baz = function(bat) {
  console.log(this.foo, bat);
};

// Initiation
const myClass = new MyClass('bar'); // create new instance
myClass.baz('bat'); // => console.log("bar", "bat");
myClass.foo = 'lol'; // => console.log("bar", "bat");
myClass.baz('what?'); // => console.log("lol", "what?");
```

- ES6 Class example:

```javascript
class MyClass {
  constructor(bar) {
    this.foo = bar;
  }

  baz(bat) {
    console.log(this.foo, bat);
  }
}
// Initiation is the same as above
```

This pattern also has a **revealing variant** which just means that not all methods are exposed:

- Revealing Prototypical example:

```javascript
// blueprint
function MyClass(bar) {
  this.foo = bar;
}

MyClass.prototype = function(bat) {
  const private = 'you don’t see me';
  const privateFunc = () => 'you also don’t see me';
  const baz = function(bat) {
    console.log(this.foo, bat);
  };
  return { baz };
};

const myClass = new MyClass('bar'); // create new instance
myClass.baz('bat'); // => console.log("bar", "bat");
console.log(myClass.private); // => undefined
console.log(myClass.privateFunc()); // => TypeError … is not a function
```

As you can see, the `private` variable is not exposed while `baz` is.

- Revealing Class example:

```javascript
class MyClass {
  #private;
  constructor(bar) {
    this.foo = bar;
    this.#private = 'you don’t see me';
  }

  baz(bat) {
    console.log(this.foo, bat);
  }

  #privateFunc() {
    return 'you also don’t see me';
  }
}
```

_Note: this is a new technology and you might need a polyfill for this to work._

### Pros and Cons of the Prototype Pattern

#### Pros

- You can clone objects without coupling to their concrete classes.
- You can get rid of repeated initialization code in favor of cloning pre-built prototypes.
- You can produce complex objects more conveniently.

#### Cons

- Cloning complex objects that have circular references might be very tricky.

## Module Design Pattern

Is the pattern of organizing code into modules that allow _encapsulation_ to have a private scope giving access to selected methods and classes to the outside.  
There is a variant to this pattern which is called the **revealing module design pattern**. Which is basically the same but instead of exposing the whole object or nothing, you just expose only some selected methods.

### Real Life Example

A real life example might be an elevator button. To us, it just exposes one method (for example floor 10) but it might rely on more private functionality in the background which is handled when pressed (close door/go to 10/open door).

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/elevator.jpg"
  alt="floor buttons in an elevator"
/></p>

### JavaScript Example

There are different ways we can use to achieve the modular pattern:

- IIFE (Immediately-invoked Function Expression)

```javascript
const myModule = (function() {
  //...
  return {
    //...
  };
})();
```

- AMD Modules

```javascript
define(
  'MyModule',
  ['RequiredModule'],
  function (RequiredModule) {
    return {
      //...
    }
  };
);
```

- CommonJS Modules

```javascript
// in uppercase.js
module.exports = str => str.toUpperCase();
// somewhere else.js
const uppercaseModule = require('uppercase.js');
uppercaseModule('test');
```

- Native JavaScript Modules

```javascript
// in uppercase.js
export default str => str.toUpperCase();
// somewhere else.js
import uppercaseModule from 'uppercase.js';
uppercaseModule('test');
```

### Pros and Cons of the Module Design Pattern

#### Pros

- Encapsulation. Which prevents variable or method names to clash and override each other.

#### Cons

- Revealing module pattern keeps private methods out of scope. That is both, good and bad. It can be good because you have the power to limit users to only use public methods. It can be bad because the user has no possibility to extend some private functionality.

## Decorator Pattern

Decorator is a pattern that lets you add new behaviors to objects by placing them inside special wrapper objects containing those new behaviors.  
The decorator pattern is sometimes also named Wrapper.  
In python for example a decorator is a function that takes another function, extending the behavior of the latter function without explicitly modifying it.

### Real Life Example

- In react we find this pattern quite often. [withRouter](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/withRouter.md) from React-Router is an example.

Wearing clothes is an example. Cold? Wrap yourself in a sweater. Rain? Put on a raincoat. These garments “extend” your basic behavior but aren’t part of you. You can take them off whenever you don’t need it.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/snow.jpg"
  alt="It snows in the city, people walk around wearing big coats"
/></p>

### JavaScript Example

- Without [decorator syntax](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)

```javascript
function admin(target) {
  target.role = 'admin';
  return target;
}

class User {
  constructor(name) {
    this.name = name;
    this.role = 'user';
  }

  say() {
    console.log(`User: ${this.name}, ${this.role}`);
  }
}

const user = new User('me');
user.say(); // User: me, user
const admin = admin(new User('him'));
user.say(); // User: him, admin
```

### Pros and Cons of the Decorator Pattern

#### Pros

- Extend an object’s behavior without making a new subclass.
- Add or remove responsibilities from an object at run time.
- Combine several behaviors by wrapping an object into multiple decorators.
- Single Responsibility Principle. You can divide a monolithic class that implements many possible variants of behavior into several smaller classes.

#### Cons

- Difficult to test the base object when there are a lot of wrappers around.
- Hard to remove specific wrapper from the wrappers stack.
- It’s hard to implement a decorator in such a way that its behavior doesn’t depend on the order in the decorators stack.
- The initial configuration code of layers might look pretty ugly.

## Adapter Pattern

Adapter is a structural design pattern that allows objects with incompatible interfaces to collaborate.

### Real Life Example

A good example for this is a universal power charger travel adapter.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/adapter.jpg"
  alt="Travel adapter to charge your devices"
/></p>

### JavaScript Example

```javascript
function getData() {
  // returns XML
  return new DOMParser().parseFromString(`<foo>bar</foo>`, 'text/xml');
}

function useData() {
  // needs JSON
  const data = xmlToJsonAdapter(getData());
  console.log('used: ', data[0].foo); // => used: bar
}

function xmlToJsonAdapter(xml) {
  // converts XML to JSON
  return [...xml.childNodes].map(node => ({
    [node.tagName]: node.textContent,
  }));
}
```

### Pros and Cons of the Adapter Pattern

#### Pros

- Single Responsibility Principle. You can separate the interface or data conversion code from the primary business logic of the program.
- Open/Closed Principle. You can introduce new types of adapters into the program without breaking the existing client code.

#### Cons

- Complexity increases because you need to introduce a set of new interfaces and classes.

## Observer Pattern

The Observer Pattern is basically a part called _subject_ that maintains a list of so called _observers_ and notifies them of changes.

### Real Life Example

A newspaper subscription could be an observer pattern. People can be added to a list, when there is a new newspaper coming out they will go through the list and send out the newspaper to each of the observers/subscribers.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/observer.jpg"
  alt="Man reading a newspaper"
/></p>

### JavaScript Example

- Talking about redux, the `connect` method is an example of an observer pattern.
- Angulars two ways binding is also an example of an observer pattern.

An observer pattern could be something like that:

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="js,result" data-user="ThibaultJanBeyer" data-slug-hash="BevZNL" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Singleton Pattern Example">
  <span>See the Pen <a href="https://codepen.io/ThibaultJanBeyer/pen/BevZNL/">
  Singleton Pattern Example</a> by Thibault Jan Beyer (<a href="https://codepen.io/ThibaultJanBeyer">@ThibaultJanBeyer</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Pros and Cons of the Observer Pattern

#### Pros

- Decouples observers from the initiator.
- Send data to many other objects in a very efficient manner.
- You can add and remove observers at anytime.
- Built in, in some languages/frameworks

#### Cons

- Coupling between the subject and its observers
- Execution order is not deterministic

## Publish Subscribe Messaging Pattern

Is a somewhat loosely coupled observer pattern: a sender of messages called _publisher_ sends _topics_ (messages) to the classes without knowing who is subscribing. _Subscribers_ listen to _topics_ and only receive messages relevant to that _topic_ without knowing about the sender.

### Real Life Example

A suggestion or complaint box could be an example. Another example might be an information window, the _publisher_ places informational messages into the window then anyone interested can read those messages.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/message-wall.jpg"
  alt="Man reading a news from newspapers hanging on the wall"
/></p>

### JavaScript Example

A Pub/Sub pattern in JavaScript could be something like this:

```javascript
const topic = {};
(function(q) {
  var topics = {},
    subUid = -1;
  q.subscribe = function(topic, func) {
    if (!topics[topic]) {
      topics[topic] = [];
    }
    var token = (++subUid).toString();
    topics[topic].push({
      token: token,
      func: func,
    });
    return token;
  };

  q.publish = function(topic, args) {
    if (!topics[topic]) {
      return false;
    }
    setTimeout(function() {
      var subscribers = topics[topic],
        len = subscribers ? subscribers.length : 0;

      while (len--) {
        subscribers[len].func(topic, args);
      }
    }, 0);
    return true;
  };

  q.unsubscribe = function(token) {
    for (var m in topics) {
      if (topics[m]) {
        for (var i = 0, j = topics[m].length; i < j; i++) {
          if (topics[m][i].token === token) {
            topics[m].splice(i, 1);
            return token;
          }
        }
      }
    }
    return false;
  };
})(topic);

// can be subscribed to like this:
topic.subscribe('api/user/refresh', user => console.log(user.name));

// topics can be published like that
topic.publish('api/user/refresh', { name: 'foo' }); // this would trigger a console.log("foo");
topic.publish('api/user/refresh', { name: 'bar' }); // this would trigger a console.log("bar");
```

### Pros and Cons of the Pub/Sub Pattern

#### Pros

- Publishers are loosely coupled to subscribers, and need not even know of their existence.
- Scalability: parallel operation, message caching, …

#### Cons

- No way to know if the message was successfully received.
- A publisher in a pub/sub system may assume that a subscriber is listening, when in fact it is not.

## Mediator Pattern

A centralized controller to which components communicate instead of directly to each other.

### Real Life Example

An airport control tower: all planes communicate to the tower, the tower then gives instructions to the planes based on the input of the various planes. They don’t talk directly to each other.

<p><img class="aligncenter size-full"
  src="{{ site.baseurl }}/assets/patterns/airport-tower.jpg"
  alt="Airport control tower with planes flying around"
/></p>

### JavaScript Example

Here is a chatroom example as mediator pattern:

```javascript
class Chatroom {
  constructor() {
    this.member = {};
  }
  register(member) {
    member[member.name] = member;
    member.chatroom = this;
    return this;
  }
  send(message, from, to) {
    if (to) return to.receive(message, from); // direct message
    Object.values(this.member) // broadcast message
      .forEach(member => member.receive(message, from));
  }
}

class Member {
  constructor(name) {
    this.name = name;
    this.room = null;
  }
  send(message, to) {
    this.room.send(message, this, to);
  }
  receive(message, from) {
    console.log(from.name + ' to ' + this.name + ': ' + message);
  }
}

const john = new Participant('John');
const doe = new Participant('Doe');
const chatroom = new Chatroom();
chatroom.register(john).register(doe);

john.send('All you need is love.'); // broadcast to everyone
john.send('I love you Doe.', doe); // private message to doe
```

In this case the chatroom is the mediator between its members.

### Pros and Cons of the Mediator Pattern

#### Pros

- Single Responsibility Principle.
- Open/Closed Principle. You can introduce new mediators without having to change the actual components.
- You can reduce coupling between various components.
- You can reuse individual components easily.

#### Cons

- Over time a mediator can evolve into an insanely big and complex Object.

## Further Reading

If you liked the patterns I can recommend following resources for a deep dive:

- [Design Patterns explained by refactoring guru](https://refactoring.guru/design-patterns) which is very well illustrated.
- [Learning JavaScript Design Patterns. A book by Addy Osmani](https://addyosmani.com/resources/essentialjsdesignpatterns/book/) which is very detailed.
- [Scotch Tutorial on 4 Patterns](https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know)
- [Do Factory](https://www.dofactory.com/javascript/design-patterns) they have a lot of patterns, unfortunately the code examples are mostly bad (because they want you to buy their book)

## Final words

Congratulations! You just learned the basics from the most used design patterns in JavaScript. This should give you an understanding of code architecture and how to improve your code. Considering patterns truly makes you stand apart from most devs and improves your code quality.

PS: don’t forget to share the knowledge and to follow me <a href="https://twitter.com/ThibaultBeyer">on twitter <i class="fab fa-twitter"></i></a> or via <a class="toggle-sidebar" data-a11y-dialog-show="subscribe-dialog" href="#site-sidebar">email <i class="fa fa-fw fa-columns"></i></a> if you liked this post :)
