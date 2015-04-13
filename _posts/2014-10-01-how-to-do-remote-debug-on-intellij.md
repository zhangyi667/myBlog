---
layout: post
title:  "How to do remote-debug on IntelliJ"
date:   2014-10-01 18:30:46
categories: work
---

Remote debugging:
When doing remote debugging in IntelliJ, you should add a new debug configuration `remote`, which would automatically fill blanks with:

`-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8000`

into the target machine.

What you need to do is :

####1, Fill in the target machine host and port. 

####2, Make sure that target machine is listening this port (because Java in default won’t allow such action).
How do I know if this port is available or how can I modify the port on target machine?

In windows it could be in the Java file in regit. In Linux, it could be in file `/etc/init.d/` 

In windows, the java file is located:

32-bit Windows: `LOCAL_MACHINE\SOFTWARE\ComputerAssociates\Identity Manager\Procrun 2.0\im_jcs\Parameters\Java`

64-bit Windows: `LOCAL_MACHINE\Software\Wow6432Node`