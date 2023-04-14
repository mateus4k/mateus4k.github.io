+++
title = "Circuit Breakers - The Complete Guide"
date = "2023-04-14"
author = "Mateus Sampaio"
description = "The circuit breaker is a microservices design pattern that improves system resilience by isolating failing services. It wraps an external service call and monitors recent failures. When the number of failures exceeds a threshold, the circuit breaker trips and gives the external service time to recover its health. This prevents cascading failures and increases overall system reliability."
+++

## TL;DR

The circuit breaker is a microservices design pattern that improves **system resilience** by isolating failing services. It wraps an external service call and monitors recent failures. When the number of failures exceeds a threshold, the circuit breaker trips and gives the external service time to recover its health. This prevents cascading failures and increases overall system reliability.

## Overview

The microservices architecture has gained popularity in recent years due to its benefits, including scalability and flexibility. However, as the number of microservices in a system increases, it can become challenging to ensure the system's reliability and resilience. One of the main challenges is dealing with the failures that can occur when communicating with other services, which can cause **cascading failures** and bring down the entire system.

Imagine this situation: "John, a software engineer, worked within a large microservices ecosystem for a global e-commerce platform that received tens of millions of daily requests. To ensure a good user experience, his APIs would retry requests when dependent services failed. However, one day, John heard that the orders service database had stopped responding. Initially, he was unconcerned, but soon noticed that some of his APIs were experiencing increased response times and failure rates. After conducting some investigation, he discovered that one of the systems he depended on utilized the system that went down. The orders service was eventually restored, but the outage lasted for hours, resulting in lost revenue and blame for related teams. In response to the incident, John and his team implemented a design pattern throughout the system, making it more resilient to failures and capable of quick recovery."

The circuit breaker pattern can **save your application's resilience** by detecting failing services and placing them in a kind of quarantine. The key is to wrap and monitor the service call. This allows us to trip the circuit breaker when the number of failures exceeds the thresholds, preventing cascading failures and giving the failing service time to recover, improving the overall reliability of the system. Therefore, this pattern is an essential tool for ensuring the resilience and availability of microservices-based systems.

{{< figure src="/circuit-breaker-overview.png" position="center" caption="Circuit Breaker in action. Credits: https://github.com/Netflix/Hystrix/wiki" >}}

## Structure

### States

One of the most important things about circuit breakers is understanding their states. When service calls are grouped by this pattern each of them will be monitored which can affect the state of the circuit.

- **Closed:** This is the default state. When everything is fine, the circuit breaker allows service calls to pass through normally.
- **Open:** This state is reached when the number of recent failures exceeds the threshold, tripping the circuit breaker. Then, it will return a fallback response or an error, without actually making the call to the service. If this were your house's circuit breaker, you'd be out of power.
- **Half-open:** To test if the service has recovered, this state allows a limited number of service calls to pass through. The circuit will revert to the closed state if the service becomes healthy again.

### Fallbacks

If you are building an e-commerce like John and your call to the personalized product recommendations service fails for some reason, you can use an alternative response with the most popular products or with recommendations stored in a stale cache. This is great for not letting the customer down, even if your API response isn't exactly what you'd like, there are still products in there.

## Implementation

You probably notice that this is a powerful tool for improving the system’s resilience. So, how to implement it?

### In-Memory

This is the most common type of circuit breaker and probably the one you will need to use. It stores the state of each breaker in the application's memory, allowing easy access to the current status the next time a service needs to be called.

There are several popular implementations, so you'll hardly need to create your own. Here are some popular ones from different programming languages:

- Node.js: [Opossum](https://github.com/nodeshift/opossum) (written by Red Hat)
- Java: [Resilience4J](https://github.com/resilience4j/resilience4j) (used by Netflix) and [Hystrix](https://github.com/Netflix/Hystrix) (Written by Netflix but currently in maintenance mode)
- Ruby: [Semian](https://github.com/Shopify/semian) (written by Shopify)
- Go: [Go-Resiliency](https://github.com/eapache/go-resiliency)
- C#: [Polly](https://github.com/App-vNext/Polly)

### Distributed

Distributed circuit breakers mean that the status of all circuits in an application will be centralized in storage outside the application, for example in a Redis instance. A good case for this type of circuit breaker is serverless applications where you cannot store a value in memory to reuse later.

Be careful when using it for a non-serverless application, especially running on multiple replicas. Grouping the status of your circuit breaker in a single location may seem like a good idea at first, because when the circuit breaker trips, all replicas would stop consuming the failing service, however, the harm that this will bring is to generate a **traffic spike** in the service after the circuit is closed again. This can cause the service, which was recovering, to drop out again, reopening the circuit once more. Instead, using an in-memory circuit breaker brings the security of a gradual return of calls to the failed service, as the load balancer will distribute the requests among the replicas. Also, going the distributed way, you will have higher complexity and latency, and the addition of a single point of failure when calling the service, so the temporary unavailability of Redis will make your circuit breaker unavailable as well.

### Service Mesh

If you are using Kubernetes, a good alternative to implementing this pattern is choosing a service mesh, like [Istio](https://github.com/istio/istio). Implementations like this use a sidecar proxy, which is an extra container inside each pod. It intercepts all service traffic and controls the circuit breaker. With it, you can get easy setup, monitoring, and a platform-agnostic solution, making it easy to adopt in a multi-language ecosystem.

However, implementing a service mesh from scratch introduces additional complexity that can negatively impact the **infrastructure cost** and **application performance**, which makes this choice more viable for ecosystems that already have a service mesh set up or that have a wide range of programming languages, including languages without support for any external circuit breaker library.

## Configuration

Finding good values for circuit breakers is complex and there is no one-size-fits-all value. A great way to discover good values is to test and learn from those who have already tested a lot, so I highly recommend reading the article [Your Circuit Breaker is Misconfigured](https://shopify.engineering/circuit-breaker-misconfigured) written by Damian Polan on Shopify, where he documents a great real-world use case.

Let's discuss each type of setting commonly found in circuit breakers and how to set good values for them:

### Max Failures Counter

The maximum number of failures of a circuit breaker refers to the number of errors until the circuit is opened. My strong recommendation is: don't use it! Absolute values can be very harmful to your project, because as the number of requests grows, the percentage of errors from a failed service will also grow, forcing you to adjust your configuration in advance, which is not always possible. Prefer to use error percentages instead of absolute values.

### Error Threshold Percentage

The expected failure rate is a value that needs to be set carefully. A value that is too high may cause the circuit not to open even when it should, and a value that is too low may cause the circuit to open unnecessarily. Observing the percentage of errors in the last 30 days of the service you are consuming can be a good basis for finding the ideal value.

Also be sure to ignore the expected errors for opening the circuit, not letting them get in the way of the actual percentage of unexpected errors. If you are consuming a search service, it is quite common for the number of 404 errors to be high, however, it makes no sense to use it to open the circuit as it is an expected error.

### Sleep Window

Also known as _reset timeout_, this is the time the circuit will be open and should correspond to the expected recovery time of the failed service. A good way is to start with high values like 30 or 60 seconds, monitor the average time until service recovery, and then decrease the timeout until you find the ideal value for your business.

In more critical scenarios you can implement an adaptive timeout based on the last N circuit closures, increasing the probability of the circuit closing faster. However, this solution brings more complexity and increases the application's memory consumption, even if insignificantly.

### Timeout

This is the amount of time the circuit breaker will wait before considering a call as a failure. A good timeout value is essential for the consumption of external services, especially if your system is a synchronous API, which needs to receive a response from another service to return it to the requesting party. In these cases, the best way to find a good timeout value is by looking at the 99th percentile of the service's response time. Setting a value much higher than this can simply destroy your application's response time, however, when the information consumed is very critical, this may be necessary.

Also check if your use case fits better with a **pessimistic** or **optimistic** timeout, as this can save your server resources. When using an optimistic strategy, for example with the [AbortController](https://nodejs.org/docs/latest/api/globals.html#globals_class_abortcontroller) or [AbortSignal](https://nodejs.org/docs/latest/api/globals.html#static-method-abortsignaltimeoutdelay) in Node.js, you determine that after the time limit is reached, the processing of waiting for that request will be ended, being great for data read-only requests. In the optimistic strategy, the request will still be running in the background, but you will ignore its return, which can be useful for requests that involve data writing, like adding a new item to the customer's wishlist, for example.

## Conclusion

Now it's up to you: test different configurations until you find the best one for your context.
