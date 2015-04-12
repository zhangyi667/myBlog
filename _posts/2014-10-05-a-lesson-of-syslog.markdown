---
layout: post
title:  "A lesson of Syslog"
date:   2014-10-05 18:30:46
categories: work
---
When re-factor the syslog module of my little project, crush happened. 

When tracked down the trace I found a weak point of my design:

When the web-application is doing initialization, some modules are loading, including the log module. And during this peroid, if any one of them fails and throws exceptions, the syslog would be initiated and write the log. But what if exceptions happen when the syslog is initiating? It would throw an exception(something like "LogInitializationFailedException"), which requires the syslog to write, so the syslog has to initiate again and the exception happens again......a loop is borned, which in the end would consume all the memory and leads StackOverFlow.

So there should be a mechanism that prevent all of these happening.

There is a simple way to do this(Why did I missed this in the first time?): We can disable the syslog until the application finishes the initialization. This is can-do because any exception-during-initialization can be logged by log4j, which means an external log system is not necessary during server initialization.

"You complete me!" "No, you are not."