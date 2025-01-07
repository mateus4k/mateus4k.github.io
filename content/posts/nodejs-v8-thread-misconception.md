+++
title = "Node.js DOES NOT use a Thread Pool for All I/O"
date = "2024-12-27"
author = "Mateus Sampaio"
+++

One of the most common misconceptions on Node.js is that all input/output operations are performed using a thread pool, and that these threads are blocked while awaiting the completion of I/O. In practice, Node.js uses operating system features whenever possible to accomplish non-blocking input/output without the need for threads. For instance, the OS provides asynchronous system calls that do not require blocking threads for networking tasks like processing HTTP requests or reading from sockets.

However, there are cases where the OS does not provide an asynchronous API, such as certain file system operations on Unix. In these situations, Node.js uses a thread pool to handle the tasks in the background. This is also true for some DNS lookups and cryptographic operations, like generating random bytes or running the pbkdf2 algorithm. Think of it like a multitasking chef: Node uses the kitchen staff (OS APIs) for most tasks, but occasionally calls in extra help (the thread pool) when the kitchen tools aren’t enough.

This balanced approach ensures high performance and scalability while maintaining Node.js’s event-driven architecture. It’s important to remember that Node.js is not just single-threaded or thread-pool-dependent—it strategically combines both to handle diverse workloads efficiently.
