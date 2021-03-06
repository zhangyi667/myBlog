---
layout: post
title:  "My experience of an ecommerce website architecture (part 1)"
date:   2015-05-17 18:30:46
categories: work
shortcut: Some experience of developing an ecommerce website.
---
Lately I’ve been developing an ecommerce website with my friends. It is a small website, which to me is more of an experience. During the development and maintenance after it went published, the website architecture has been evolved, which I think is something that worthy to share with.

There are two reasons that I share my experience. First, I know for e-commerce website there are already some mature/classic architectures to implement. However, it is used in companies of large number of users and/or large bunch of data to deal with. To start with a small website you might encounter some issues that you would never expect to meet in big companies. Also with the number of users and transactions increases, you definitely wanna change the architectures, and during which I have figured out the necessity of some certain architectures.

##### The architecture of website.

Since it’s just a start-up website, the sub-services of which are very simple: Website Frontend, System Management, app.

What we used initially was LB + MVC + cache + database.

<img src="http://oa1f2pgjm.bkt.clouddn.com/blog/e-commerce-1.png" width="80%">

##### Log and monitor system

We built a log system before published. And used log to estimate the input and output. A few weeks later when the server went steady, it was time to design a monitor system to monitor all resources.

What we decided was to monitor these modules: resources, servers, services, exceptions and application performance. 

Resource: CPU, Memory, threads, etc. 

Memory Usage: 

	'(MemoryMXBean)ManagementFactory.getMemoryMXBean().getHeapMemoryUsage().getUsed();'

Thread:

	'(ThreadMXBean)ManagementFactory.getThreadMXBean().getThreadCount();'

CPU usage:

	'(OperatingSystemMXBean)ManagementFactory.getOperatingSystemMXBean().getProcessCpuLoad'

Servers: To check the availability of all servers. Had any server went offline, system sent developer group an email.

Service: To check the availability of all services, as api service, management console service and photo service.

Application performance: to check performance of application. At beginning we just checked the QPS of API service. If during some time QPS drops or raises sharp, the system sends developer group an email.

My experience: Do not develop all the functions at a time. Rome was not built in a day. First make it work, then consider how to improve it.

##### Log:
It was until something went wrong and you couldn’t locate error that you realise the importance of log. In most cases, you couldn’t locate the error because you forget to log one variable, which cost you extremely long time to solve it.

From my experience, there are some standards of log. When you review your code, please remind yourself if :

* _The log records much enough. Exception, external call, the input, the output and all the key variables._

* _The log contains enough info, which includes context, as well as the return value of all external methods. And also some id words that helps distinguish itself from others._

* _Since you can set log level as info/debug/error before publish your system, so the number of logs is not that important. As long as the log codes are not too much to affect transaction codes, all are acceptable._

After designed the monitor module, the architecture looks like:

<img src="http://oa1f2pgjm.bkt.clouddn.com/blog/e-commerce-2.png" width="80%">

##### Database Cluster:

In the beginning it was only one database server, which was in the same VPS with api service. It was not good but at that time we were busy to publish it. 

After one or two weeks, we updated it by applying a new Database server.

Not a few weeks later, we found that the database was the bottleneck, because we didn’t implement cache and every request cost at least one query of Database. 

Then we implemented cache, and master-slave mechanism to raise the query speed and back up the data.

Then to avoid single point failure, we implemented master-master, and load balance for databases.

The last step we segregated read and write databases. 6 for read and 2 for write. 

After implemented Database cluster, the architecture looks like:

<img src="http://oa1f2pgjm.bkt.clouddn.com/blog/e-commerce-3.png" width="80%">

##### Photo: From photo server to cloud server
The e-commerce website requires a large demand of photo service, where sellers can upload the product photos. In the beginning we developed a resource server which was basically a photo server.  Not long later it became the bottleneck because when user uploaded pictures, it requests api servers and waits for response, which includes I/O read and write so it was very slow. 

Then we analysed the demand of pictures and tried to make a change.

We found that when sellers uploading pictures, as long as they’re sure the pictures has been uploaded, they can accept a little bit delay. Thus we improved the way to upload picture to implement asynchronous operation. When user uploads pictures, we add them into a queue and return. Another service, picture handler listens to this queue, and uploads them to a third party cdn service. When uploads succeeds, it updates the database, else send error message to that user. From user side, when he uploads pictures, he sees the response from server immediately and after a few seconds he can see them in the product page, or if the uploading failed, he saw error messages.  Moreover, since only sellers can upload pictures, we added the limit that for one product sellers can only upload or modifies the pictures every certain time, to reduce the requests to api server.

After segregate photo service from api service, it looks like:

<img src="http://oa1f2pgjm.bkt.clouddn.com/blog/ecommerce-system.png" width="80%">

Only until you jump into it did you realise how much to consider when it comes to architecture. There's no certain types, all you need to do is to decide the best way to solve the problem based to the very certain case. This is a long way to go, and I'm happy with it.