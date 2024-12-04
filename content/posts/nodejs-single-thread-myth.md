+++
title = "Node.js: The Myth of the Single Thread"
date = "2024-12-18"
author = "Mateus Sampaio"
+++

A common misconception about Node.js is that it is *single-threaded*. Despite running JavaScript code on a single thread, it uses an *asynchronous* and *non-blocking* programming model that enables it to manage numerous concurrent connections and efficiently carry out I/O activities.

The **V8** engine is a core component of Node.js and runs JavaScript code on a single thread. But the **libuv** library and the **event loop** are where the true magic is. The event loop continuously monitors an **event queue**, processing different phases such as timers, I/O callbacks, and completed asynchronous tasks. Libuv controls a **thread pool** to handle operations that cannot be performed asynchronously by the operating system, such as file system and DNS tasks, while abstracting system calls.

In conclusion, Node.js’s asynchronous paradigm, combined with the libuv thread pool, makes it a powerful runtime for building scalable applications that efficiently handle concurrent connections and I/O tasks.
