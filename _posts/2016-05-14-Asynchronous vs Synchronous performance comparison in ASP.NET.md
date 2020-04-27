---
title: "Asynchronous vs Synchronous performance comparison in ASP.NET"
date: 2016-05-14 11:29:00 -0400
tags: [asynchronous, aync, await, asp.net, mvc, webapi, api]
---
Asynchronous programming is recommended pattern to scale performance of applications due to its non-blocking I/O characteristic ([read more](https://docs.microsoft.com/en-us/dotnet/standard/async-in-depth#deeper-dive-into-tasks-for-an-io-bound-operation){:target="_blank"}). In this article, we'll compare performance of asynchronous vs synchronous request processing in an ASP.NET MVC application to quantify its performance improvement.

## Setup
3 servers are configured to run (i) a load tester, (ii) an ASP.NET MVC application having asynchronous and synchronousversions of a method that requests data from back-end service (iii) a Node.JS backend service that responds after specified delay. Concurrent requests ranging 10 to 100 are invoked from load tester for synchronous method of the ASP.NET MVC application followed by asynchronous method invocations. Test is repeated by changing delay in Node.JS application to 0 ms, 100ms, 250ms & 500ms. Throughput, response time and server resources utilized by ASP.NET application are measured using performance counters.

## Throughput
First performance metric we'll look at is throughput for different latencies of backend service. Throughput naturally decreases as latency increases but asynchronous requests delivers 1.4x, 4.0x, 5.8x & 6.5x more throughput than synchronous requests.

![throughput-latency](/assets/images/throughput-latency.png)
##### Figure 1: Throughput v/s latency

Another scenario is measuring throughput for various levels of concurrency. In this case we'll use fixed latency of 100ms for the backend service so that it does not affect performance and make 10 through 100 concurrent requests. Throughput for synchronous requests is almost the same for any level of concurrency whereas it increases linearly for asynchronous requests with maximum throughput at concurrency of 80. Server is fully saturated at this level (max performance) after which throughput starts decreasing, though its significantly higher than synchronous requests.

![throughput-concurrency](/assets/images/throughput-concurrency.png)
##### Figure 2: Throughput v/s Concurrency

This throughput behavior correlates with hyper threaded quad-core processor of the server running ASP.Net application which can perform 8 tasks in parallel (4 cores x 2 hyper threading). In case of synchronous requests only 8-9 requests execute concurrently since threads are blocked and remaining requests are queued (as shown in white bar). Threads are not blocked for asynchronous requests hence all requests execute with queue almost empty.

![executing-requests](/assets/images/executing-requests.png)
##### Figure 3 Executing vs Queued Requests

## Response Time
Response time is the time taken to process a request; usually denoted in milli-sec (ms). Figure 4 shows response time taken by synchronous and asynchronous requests for various I/O duration. Average response time of synchronous request processing deteriorates as I/O takes longer due to requests waiting to be processed; whereas asynchronous request processing shows improved average response time. As seen in the figure overhead to process asynchronous requests is 173.7 ms for I/O latency of 0 ms, which reduces to 104.6 ms (204.4 - 100), 64.1 ms (314.1 - 250) and 42.7 ms (542.7 - 500) as I/O latency increases.

![performance-latency](/assets/images/performance-latency.png)

##### Figure 4

Also as shown in Figure 5, average response time of synchronous request processing increases proportionally to increase in concurrency of requests; where asynchronous request processing exhibits much better response time for any level of concurrency. Better response time of asynchronous processing is again due to the fact that requests were not queued and hence had no wait time before being processed. It also increases with increasing concurrency but at a very small rate, which is obvious due to more requests, more I/O, etc. being handled by the application.

![performance-concurrency](/assets/images/performance-concurrency.png)

##### Figure 5

Though average response time is used as a metric above, best practice for measuring response time is 95th percentile since it also depicts spikes or outliers that are going to be experienced. Average response time of synchronous requests is closer to max, indicating majority of requests taking longer to process then they should have. Average response time of asynchronous requests either coincides with median or closer to minimum response time indicating consistent response time with no major spikes.

![response-time-distribution](/assets/images/response-time-distribution.png)

##### Figure 6

##Resource Consumption
CPU utilization and memory consumption are among the major system resources and hence are measured to determine efficiency of the request processing patterns. As shown in figure 7, cumulative CPU utilization of asynchronous requests is higher than synchronous request processing due more requests are being processed. This in fact is better utilization of server resources since CPU is being consumed instead of being idle.

![cpu](/assets/images/cpu.png)

##### Figure 7

Memory consumed by asynchronous requests is ~30% higher than synchronous request processing and hence needs to be taken into consideration in physical resource specification but higher throughput of asynchronous requests surpasses higher memory consumption.Figure 8 shows memory consumption of both request processing patterns.

![memory](/assets/images/memory.png)

##### Figure 8

## Conclusion
Every web application performs some or the other kind of I/O (external api calls, database requests, etc) and hence asynchronous request processing, which is programatically simpler to implement can be leveraged to build high performance web applications. Merely due to unblocked threads, more requests are processed with negligible queuing which yields higher throughput, consistent response time and better server resource utilization.
