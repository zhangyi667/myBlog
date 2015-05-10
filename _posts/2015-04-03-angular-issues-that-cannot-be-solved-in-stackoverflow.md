---
layout: post
title:  "Angular Issues whose answers cannot be found in StackOverFlow"
date:   2015-04-03 18:30:46
categories: work
shortcut: The StackOverFlow is not omniscient....
---
I’ve been learning angularJs these days, and is doing a new web application project.
In the past week, the framework of backends was under discussed, as well as the cloud server. Now since it’s done, I’m starting to do front end.

The first thing I started to do, of course, was to set up all the groundwork. Surely the play-angular framework was brief and beautiful, however, since i'm using google App-Engine, the front end has to be a “classic” combination of AngularJs+grunt+karma+nodeJs+whatever necessary.

Anyway, when I use grunt to start the angularJs service, it aborted due to such error: 

`RangeError: Maximum call stack size exceeded`.

My first guess was that the memory is the key to solve it. DO NOT trust the stackoverflow, [this][firstS] and [this][secondS] were not even close to the answer.

In [github discussion group][group] someone encountered the same issue and solved it by increasing memory. And this command finally helped me out of the hell:

`echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p`

Command “tee” receives standard input and output it to standard output device, meanwhile saves it as a file. “-a” means to append the content in the end of file instead of overriding it.

Command “sysctl” is to configure kernel parameters at runtime, which executes the sysctl.conf file as default. 

So this command means, first append a sentence “fs.inotify.max_user_watches=524288” in file sysctl.config, and re-configure kernel parameters according to the sysctl.config file.

[firtS]:http://stackoverflow.com/questions/22285942/grunt-throw-recursive-process-nexttick-detected
[secondS]:http://stackoverflow.com/questions/22644709/warning-recursive-process-nexttick-detected
[group]:https://github.com/yeoman/generator-webapp/issues/396