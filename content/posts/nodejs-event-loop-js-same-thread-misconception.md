+++
title = "Node.js DOES NOT run the Event Loop and V8 on separate threads"
date = "2025-01-07"
author = "Mateus Sampaio"
+++

A prevalent misunderstanding regarding Node.js is that the event loop operates in a separate thread from the JavaScript engine (V8). In truth, both the execution of JavaScript and the event loop operate on the same thread. This common thread underpins the single-threaded, non-blocking structure of Node.js.

Think of it like a chef in a small kitchen. The chef (JavaScript) cooks one dish at a time but also checks orders, sets timers, and calls for ingredients (asynchronous I/O operations). It’s all done by the same person in the same space, with tasks coordinated efficiently to keep the kitchen (application) running smoothly.

{{<figure src="/nodejs-event-loop-js-same-thread-misconception.png" position="center" width="300">}}

Node.js excels at handling asynchronous I/O, but blocking the thread with heavy computations or synchronous code can freeze the entire application. Both the event loop and JavaScript execution depend on this single thread, making it essential to write non-blocking code for optimal performance.
