+++
title = 'The Event Loop of Node.js'
date = 2024-05-18T16:55:21+08:00
draft = false
tags = ['Node.js', 'Event Loop']
+++

# The Event Loop of Node.js

The event loop is a fundamental concept in Node.js that enables non-blocking I/O operations, despite JavaScript being single-threaded. Here’s a high-level overview of how it works:

## Initialization:

When Node.js starts, it initializes the event loop and processes the input script, which may include asynchronous API calls, timer scheduling, or process.nextTick() calls.

Phases of the Event Loop:

- **Timers:** This phase executes callbacks scheduled by setTimeout() and setInterval().
- **Pending Callbacks:** Executes I/O callbacks deferred to the next loop iteration.
- **Idle, Prepare:** Internal phase used for preparing next steps.
- **Poll:** Retrieves new I/O events and executes related callbacks, except for close callbacks, those scheduled by timers, and setImmediate(). Node.js may block here when appropriate.
- **Check:** setImmediate() callbacks are executed here.
- **Close Callbacks:** Executes callbacks for some close events, like socket.on('close', ...).

## FIFO Queue:

Each phase has a FIFO (First-In-First-Out) queue of callbacks to execute. Node.js will process the queue until it’s empty or reaches a set limit of callbacks per phase.

## Offloading Operations:

Whenever possible, operations are offloaded to the system kernel, which is multi-threaded and can handle multiple operations in the background. When an operation completes, the kernel informs Node.js, and the callback is queued to be executed.

## Poll Phase Peculiarities:

The poll phase can run longer than a timer’s threshold if long-running callbacks are present, as new events can be queued while polling events are being processed.

The event loop allows Node.js to handle many operations concurrently, making it highly efficient for I/O-heavy workloads.

```Javascript
while(!shouldExit) {
    processEvents();
}
```

Here’s a simple code snippet to illustrate the event loop in action:

```JavaScript

console.log("First statement");

setTimeout(function() {
console.log("Second statement");
}, 1000);

console.log("Third statement");
```

Output:

> First statement
> Third statement
> Second statement

In this example, “First statement” and “Third statement” are logged immediately, while “Second statement” is logged after a 1-second delay, demonstrating the non-blocking nature of the event loop.
