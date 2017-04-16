---
layout: post
title:  "How to write logs"
date:   2015-05-03 11:30:46
categories: work
shortcut: Do you know how to write logs of a server?
---
It was until something went wrong and you couldn’t locate error that you realise the importance of log. In most cases, you couldn’t locate the error because you forget to log one variable, which cost you extremely long time to solve it.

From my experience, there are some standards of log. When you review your code, please remind yourself if :

The log records much enough. Exception, external call, the input, the output and all the key variables.

The log contains enough info, which includes context, as well as the return value of all external methods. And also some id words that helps distinguish itself from others.

Since you can set log level as info/debug/error before publish your system, so the number of logs is not that important. As long as the log codes are not too much to affect transaction codes, all are acceptable.