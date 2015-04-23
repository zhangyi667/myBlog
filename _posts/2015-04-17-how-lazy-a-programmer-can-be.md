---
layout: post
title:  "How lazy a programmer can be?"
date:   2015-04-17 18:30:46
categories: work
shortcut: I have been in touch with Google App Engine for a while....
---

I’ve been in touch with Google App Engine for a while. It is a very good, clean and efficient platform, except when it comes to developing, it is crucial. Debugging is in hell since the service runs in the Docker’s virtual machine and in the mean time our project is a gradle-angularJs project. When I have developed a new feature, I have to compile the code all over(which takes me 10 seconds), deploy it to Docker(which takes me 10 seconds) and refresh the page. I am a front end developer as you might know... For every tiny little things that I have done, I have to wait 20 seconds to see the changes. It felt like I was in 2000s when real-time debugging was like a joke...

After a whole day’s desperation, I decided to make some changes. Luckily I found it can-do. You see, when compiling all the source code, what gradle does is to analyze the dependencies, compile the classes, wrap the codes and deploy to Docker. For Java Classes, the compilation is necessary, but for js/css/html files you don’t do that. Gradle just simply copy them to a directory “exploded-app”. So technically if I just develop my own code in this director, I can see the changes in real-time. 

Problem solved, huh?
However, since all the changes are in the different directory, I have to copy them one by one. And to make sure the updated code works, I have to wait another 20 seconds to compile. Is there anything we can do to improve it?

Of course we can. [Rsync][rsynclink] is a widely-used utility to keep copies of a file on two computer systems the same, which in the meantime, definitely can sync two directories in one computer. By using this, I can write my code where I should write, and sync my code’s directory with the exploded directory by just one command. Then after refreshing my browser I can see the changes. Only one command on the terminal, which costs me only 1 second.

I thought that was it, until I saw what my colleague did...
There is an amazing app in linux called [incron][icronlink], whose job, in a classic scenario, is to listen a certain change of a file, and then automatically executes some script.

My colleague uses Incron to listen any change of the application’s directory, and then automatically sync the two directories...he doesn’t even need to type the command...

[rsynclink]: http://en.wikipedia.org/wiki/Rsync
[icronlink]: http://linuxaria.com/article/incron-cron-inotify