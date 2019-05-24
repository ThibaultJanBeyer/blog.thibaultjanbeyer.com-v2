---
layout: post
title: A Quick guide to the JavaScript Event Loop (Browser)
excerpt: TL;DR ! The JavaScript event loop. What is it? Explained in a nutshell.
image: /assets/event-loop/turbo-javascript-event-loop-roller-coaster.jpg
image_alt: A picture of a looping on a roller coaster ride with a big sign reading Turbo
---

I finally found some time to write a new article. Lately I had a talk about IT and I realized that while I know most of the topics and how they work, I have really hard times explaining them. So I looked online on how others do it and only found very huge and deep articles on those topics. So, what I want to achieve is to create very brief explanation that give you a quick and good overview about complex topics. These articles are perfect to bookmark and review in case you need to refresh your knowledge. I’ll also add resources in case you want to dig deeper. Today we will have a look at the JavaScript event loop in the browser. Enjoy reading!

## How does the event loop look like ?

Here is my representation of the event loop:

![The JavaScript Browser Event Loop](/assets/event-loop/JavaScript-Event-Loop.png)

As you can see there is a loop list, a heap which is just an allocation of memory somewhere there also resides the Web API that will handle our asynchronous activities. Then we have a stack where the event loop will place and handle our synchronous calls and a message queue where the asynchronous callback handlers will be placed.

Let me explain some concepts here quickly:

## What is a Stack ?

A stack is a conceptual structure in which elements can be placed. The concept of a stack is `FILO` which means that the first element which you place in a stack is the last element that will get out of the stack.  
A real world example of a Stack would be a PEZ Dispenser (if you don’t know what that is, I found a [PEZ video tutorial](https://www.youtube.com/watch?v=YIAN6eKltsA) online.

![Star Wars PEZ dispenser](/assets/event-loop/PEZ-dispenser-star-wars.png)

## What is a Queue ?

I think that you are all aware about what a queue is. It is a conceptual structure following the `FIFO` concept, which means that the first element which you place in a queue is the first element that comes out.  
There are plenty real world examples of a queues. For example when you do your groceries, the line in which you stay for the checkout is a queue.

![People waiting in line at a grocery store](/assets/event-loop/People-waiting-in-line-with-shopping-baskets-at-grocery-store.jpg)

## 1. The Call Stack

You JavaScript code will be parsed one after another and added to the Stack. Let’s suppose you have following code:

```javascript
function foo() {
  return 1 + 1;
}

function bar() {
  console.log(1);
  const second = foo();
  console.log(second);
}

bar();
```

Considering our representation above, this is what would happen when executing the code:

- Start code: `[main()]`.
- Push `bar()` to the stack: `[bar(), main()]`.
- Push `console.log(1)` to the stack: `[log(1), bar(), main()]`.
- Pop (resolve) `console.log(1)` from the stack: `[bar(), main()]`. => log 1
- Push `foo()` to the stack: `[foo(), bar(), main()]`.
- Pop (resolve) `foo()` from the stack: `[bar(), main()]`. => return 2
- Push `console.log(second)` to the stack: `[log(2), bar(), main()]`.
- Pop (resolve) `console.log(2)` from the stack: `[bar(), main()]`. => log 2
- Pop (resolve) `bar()` from the stack: `[main()]`.
- Pop (resolve) `main()` from the stack: `[]`.
- Exit the stack

Here is an excerpt from a [talk on the JS Conf](https://www.youtube.com/watch?v=8aGhZQkoFbQ) which explains it quite well:

![Stack Example JSConf](/assets/event-loop/stack-example-jsconf.gif)

## 2. The heap

The heap is just an unstructured region of memory that is used for execution.

## 2b. Web API

Are handling the asynchronous operations, they are responsible to hold timeouts, promises etc. until they get resolved and push the callback function to the message queue. More on that below:

## 3. The Message Queue

When we use an asynchronous method, the web-api will take care of it’s delay time and place it into the queue as soon as it is resolved.  
When the Call Stack is empty, the message queue is resolved.

Lets consider this example:

```javascript
console.log(1);

setTimeout(function callback() {
  console.log(3);
}, 5000);

console.log(2);
```

For comparison I will represent the Web API as brackets `()` and the queue like this `<>`. This is by no means how a queue or api looks like.

- Start code: `[main()]` / `()` / `<>`.
- Push `console.log(1)` to the stack: `[log(1), main()]` / `()` / `<>`.
- Pop (resolve) `console.log(1)` from the stack: `[main()]` => log 1 / `()` / `<>`.
- Give `setTimeout` to the Web API: `[main()]` / `(setTimeout[5s])` / `<>`.
- Push `console.log(2)` to the stack: `[log(2), main()]` / `(setTimeout[5s])` / `<>`.
- Pop (resolve) `console.log(2)` from the stack: `[main()]` => log 2 / `(setTimeout[5s])` / `<>`.
- Pop (resolve) `main` from the stack: `[]` / `()` / `<>`.
- Exit the stack
- The Web API timeout is over. It enqueues the `callback` function to the Message Queue: `[]` / `()` / `<callback()>`.
- _As the Stack is empty_, the message queue is processes and dequeues `callback` by pushing it to the Stack: `[callback()]` / `()` / `<>`.
- Push `console.log(3)` to the stack: `[log(3), callback()]` / `()` / `<>`.
- Pop (resolve) `console.log(3)` from the stack: `[callback()]` => log 3 / `()` / `<>`.
- Pop (resolve) `callback` from the stack: `[]` / `()` / `<>`.
- Exit the stack
- The queue is empty: `[]` / `()` / `<>`.
- Exit the queue

Here is an excerpt from a [talk on the JS Conf](https://www.youtube.com/watch?v=8aGhZQkoFbQ) which explains it quite well:

![Queue Example JSConf](/assets/event-loop/queue-example-jsconf.gif)

## (non) Blocking

As mentioned before that refers to JavaScript handling events sequentially. The call stack can not be interrupted while running. The Queue is only processes when the call stack is empty. The timer on the setTimeout is just the minimum time to wait, it can take longer to be executed since it has to wait until the call stack is empty and all entries that are in the queue before the timeout dequeued.  
So the calls executed on the web-api are referred as `non blocking`. Anything executed on the call stack is considered `blocking` as it prevents everything else from being executed. For example if you have a long running function on the call stack, it will even prevent user interaction like even clicking on buttons on the page. An infinite loop for example will block the execution of anything else forever and thus crash the browser tab.

## Best Practice

- Try to avoid long running (blocking) methods by reducing the time complexity (for example avoiding unnecessary loops).
- If the long running method can not be avoided, delegate it to a [web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers). Those will be executed in background threads and thus not block the main execution.
- Avoid infinite loops. This might sound like a no-brainer but it’s still worth mentioning, and I think that now you understand why infinite loops are crashing the webapp.

## Further reading

I hope that this breaf overview could give you a solid understanding of the JS Event Loop. If you’re feeling like you want to dig deeper, here are some articles I recommend on the topic:

- I can really recommend checking out the full [talk by Philip Roberts on youtube](https://www.youtube.com/watch?v=8aGhZQkoFbQ). I was at the JS Conf in person and was quite impressed on how well he explained the topic.
- You’ll find an on the point explanation of the [Concurrency Model and the Event Loop on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) as well.
- [Flavio Copes](https://flaviocopes.com/javascript-event-loop/#blocking-the-event-loop) digs a bit deeper in the subject then I did.
- For the NodeJS event loop, while it is similar at the core, it will still be treated a bit differently. I can recommend [Deepals 6 article series](https://jsblog.insiderattack.net/event-loop-and-the-big-picture-nodejs-event-loop-part-1-1cb67a182810) which is very detailed.

Thank you for reading and feel free to ask any question or let me know if you found any mistake.

PS: don’t forget to share the knowledge and to follow me <a href="https://twitter.com/ThibaultBeyer">on twitter <i class="fa fa-twitter"></i></a> or via <a class="toggle-sidebar" data-a11y-dialog-show="subscribe-dialog" href="#site-sidebar">email <i class="fa fa-fw fa-columns"></i></a> if you liked this post :)
