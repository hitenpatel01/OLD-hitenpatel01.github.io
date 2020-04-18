---
title: "Performance comparison between Asynchronous & Synchronous Request Processing in ASP.NET"
date: 2016-05-14 11:29:00 -0400
tags: [asynchronous, aync, await, asp.net, mvc, webapi, api]
---
Performance is a key feature of modern web applications and non-blocking I/O characteristic of asynchronous request processing is known to benefit scalability and response time of web applications. In this article, we'll compare performance of asynchronous request processing with synchronous request processing for understanding its benefits to built high performance ASP.NET MVC applications.

## Synchronous vs Asynchronous Request Processing
In synchronous request processing, a thread is assigned to a request and is responsible for its complete execution. Such threads are blocked (does nothing until response is received) whenever I/O is performed. Also the threads are assigned from Thread Pool and are limited in number, hence only a certain number of requests can execute and remaining requests are queued until threads free up. Below is typical implementation of synchronous request processing in ASP.NET MVC:
```csharp
public IActionResult Index()
{
  //Thread will block until GetData() executes
  var data = ExternalService.GetData(); 
  return View(data);
}
```
Asynchronous request processing on other hand relieves threads that are processing requests when I/O is performed and hence they are available to process other requests. Processing of the request is resumed by any available thread (from Thread Pool) once response from I/O is received. The request takes about the same time to complete but more requests can be executed due to availability of threads. Typical implementation of Asynchronous request processing is as below:
```csharp
public async Task<IActionResult> IndexAsync()
{
  //Thread will be returned to pool & execution will
  //resume once data arrived from external service
  var data = await ExternalService.GetData();
  return View(data);
}
```
## Configuration
3 servers are configured to run (i) a load tester, (ii) an ASP.NET MVC application having synchronous and asynchronous versions of a method that requests data from back-end service and returns it (iii) a Node.JS backend service that responds after specified delay. Concurrent requests ranging 10-100 are invoked from load tester for synchronous method of the ASP.NET MVC application followed by asynchronous method invocations. Test is repeated by configuring Node.JS application to delay response by 0 ms, 100ms, 250ms & 500ms. Throughput, response time and server resources are measured and compared.

## Throughput
Throughput denotes amount of work done in a unit time; typically represented as requests per second (request/sec). Figure 1 shows throughput of synchronous and asynchronous requests for various I/O duration. Throughput decreases as the duration increases for both types of request processing patterns but asynchronous requests yields higher throughput. It performed 1.4x, 4.0x, 5.8x & 6.5x more throughput than synchronous requests for I/O responding in 0 ms, 100 ms, 250 ms and 500 ms respectively.

![throughput-latency](/content/images/2017/01/throughput-latency.png)

##### Figure 1

Another scenario is measuring throughput for different number of concurrent requests. Figure 2 shows throughput for various levels of concurrency for fixed I/O duration of 100ms. Throughput for synchronous requests is almost the same for any level of concurrency but throughput for asynchronous requests increases linearly with maximum throughput at concurrency of 80. Server is fully saturated at this level hence increasing concurrency further starts dropping the throughput.

![throughput-concurrency](/content/images/2017/01/throughput-concurrency.png)

##### Figure 2

As shown in Figure 3, synchronous processing executes same number of requests which is based on number of threads in Thread Pool with remaining requests being queued (as shown in white bar) and if queue becomes full then server will respond HTTP 503 Service Unavailable. In asynchronous requests, since threads are not blocked during I/O they execute other requests and hence queue is almost empty.

![executing-requests](/content/images/2017/01/executing-requests.png)

##### Figure 3

## Response Time
Response time is the time taken to process a request; usually denoted in milli-sec (ms). Figure 4 shows response time taken by synchronous and asynchronous requests for various I/O duration. Average response time of synchronous request processing deteriorates as I/O takes longer due to requests waiting to be processed; whereas asynchronous request processing shows improved average response time. As seen in the figure overhead to process asynchronous requests is 173.7 ms for I/O latency of 0 ms, which reduces to 104.6 ms (204.4 - 100), 64.1 ms (314.1 - 250) and 42.7 ms (542.7 - 500) as I/O latency increases.

![performance-latency](/content/images/2017/01/performance-latency.png)

##### Figure 4

Also as shown in Figure 5, average response time of synchronous request processing increases proportionally to increase in concurrency of requests; where asynchronous request processing exhibits much better response time for any level of concurrency. Better response time of asynchronous processing is again due to the fact that requests were not queued and hence had no wait time before being processed. It also increases with increasing concurrency but at a very small rate, which is obvious due to more requests, more I/O, etc. being handled by the application.

![performance-concurrency](/content/images/2017/01/performance-concurrency.png)

##### Figure 5

Though average response time is used as a metric above, best practice for measuring response time is 95th percentile since it also depicts spikes or outliers that are going to be experienced. Average response time of synchronous requests is closer to max, indicating majority of requests taking longer to process then they should have. Average response time of asynchronous requests either coincides with median or closer to minimum response time indicating consistent response time with no major spikes.

![response-time-distribution](/content/images/2017/01/response-time-distribution.png)

##### Figure 6

##Resource Consumption
CPU utilization and memory consumption are among the major system resources and hence are measured to determine efficiency of the request processing patterns. As shown in figure 7, cumulative CPU utilization of asynchronous requests is higher than synchronous request processing due more requests are being processed. This in fact is better utilization of server resources since CPU is being consumed instead of being idle.

![cpu](/content/images/2017/01/cpu.png)

##### Figure 7

Memory consumed by asynchronous requests is ~30% higher than synchronous request processing and hence needs to be taken into consideration in physical resource specification but higher throughput of asynchronous requests surpasses higher memory consumption.Figure 8 shows memory consumption of both request processing patterns.

![memory](/content/images/2017/01/memory.png)

##### Figure 8

## Conclusion
Every web application performs some or the other kind of I/O (external api calls, database requests, etc) and hence asynchronous request processing, which is programatically simpler to implement can be leveraged to build high performance web applications. Merely due to unblocked threads, more requests are processed with negligible queuing which yields higher throughput, consistent response time and better server resource utilization.
