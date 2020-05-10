---
title: "Asynchronous vs Synchronous performance comparison in ASP.NET"
date: 2016-05-14 11:29:00 -0400
tags: [asynchronous, aync, await, asp.net, mvc, webapi, api]
---
Asynchronous programming is a recommended pattern to scale performance of applications due to its non-blocking I/O characteristic ([read more](https://docs.microsoft.com/en-us/dotnet/standard/async-in-depth#deeper-dive-into-tasks-for-an-io-bound-operation){:target="_blank"}). In this article, we'll compare performance of asynchronous vs synchronous request processing in an ASP.NET MVC application to quantify performance gain from asynchronous request processing.

## Setup
1. Load Tester, a multi threaded application like Apache Bench that can invoke concurrent requests
2. ASP.NET MVC application having asynchronous and synchronousversions of a method requesting data from a back-end service
3. Node.JS backend service that responds HTTP 200 OK after specified delay.

Latency in NodeJs application is set to 100ms and concurrent requests ranging 10 to 100 requests are invoked from load tester for synchronous method of the ASP.NET MVC application followed by asynchronous method invocations. Throughput, response time and server resources utilized by ASP.NET application are measured using performance counters.

## Throughput
First performance metric we'll look at is throughput (amount of requests processed in a unit of time). Throughput for synchronous requests is nearly same for any level of concurrency whereas it increases linearly until server reaches max performance and then starts to decrease for asynchronous requests.

![throughput](/assets/images/Throughput.png)

Throughput for asynchronous requests is significantly higher than synchronous requests contributed by non-blocking threads, which can be confirmed by observing number of queued requests. .Net Thread Pool determines optimal number of threads for running an application and are limited. Because of this only some synchronous requests can execute and remaining are queued. The same limited number of threads are utilized in non-blocking way for asynchronous requests and hence there are almost no queued requests.

![executing-requests](/assets/images/Queued-Requests.png)

## Response Time
Another metric is Response time (time taken to process request) which naturally increases proportional to concurrent requests for both types of requests but asynchronous requests exhibits way better performance since the requests didn't had to wait for threads to be available for processing.

![response-time](/assets/images/Response-Time.png)

## Resource Consumption
CPU utilization and memory consumption are common system resources and hence are measured to determine efficiency of the request processing patterns. Cumulative CPU utilization of asynchronous requests is higher than synchronous requests due more requests being processed. This in fact is better utilization of server resources since CPU is being consumed instead of being idle.

![cpu](/assets/images/CPU.png)

Memory consumed by asynchronous requests is ~30% higher than synchronous request processing which should be taken into consideration but benefit of higher throughput of asynchronous requests surpasses higher memory consumption.

![memory](/assets/images/Memory.png)

## Conclusion
Every web application performs some or the other kind of I/O (external api calls, database requests, etc) and hence utilizing asynchronous request processing can deliver higher performance.
