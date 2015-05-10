---
layout: post
title:  "Two way to solve angularJs Java Version Conflict"
date:   2015-04-09 18:30:46
categories: work
shortcut: I have been trapped into another stupid, stupid mistake today...
---
I have been trapped into another stupid, stupid mistake today.

Here is the thing: In the past few months, I’ve been maintaining a web-application project, which is a java project, compiled by Java 6. 

And the IDE is IntelliJ, and usually I open my Intellij on terminal.

Now:
I’m doing a new web-application project, which is a gradle project. I’m still using the same Intellij as IDE. When opening the project, error occured: `Unsupported major.minor version 51.0`.

The number describes the version of JRE the java class file is compatible with:


>J2SE 8 = 52,
J2SE 7 = 51,
J2SE 6.0 = 50,
J2SE 5.0 = 49,
JDK 1.4 = 48,
JDK 1.3 = 47,
JDK 1.2 = 46,
JDK 1.1 = 45.


So clearly this means that my IDE(using Java 6) is too old for the current gradle version, which requires Java 7. However, if I changed $JAVA_HOME in env, I cannot compile my previous web-application.

How did I fixed it?
The key to this problem is that it was not a conflict. In fact there are many ways to solve it, I'd like to share my experience as well as two solutions.

1, Taking a glance at linux desktop files, which is something like:

`[Desktop Entry]
Name=IntelliJ14
Comment=
Exec=env -u XMODIFIERS JAVA_HOME=/usr/java/jdk1.7.0_40 /home/idea-IU-139.659.2/bin/idea.sh
Icon=/home/idea-IU-139.659.2/bin/idea.png
Terminal=false
Type=Application
Encoding=UTF-8
Categories=Development;
`

The rest are not important, take care of “Exec”, which is the bash command to boot the process.

`env -u XMODIFIERS JAVA_HOME=/usr/java/jdk1.7.0_40 /home/xx/idea-IU-139.659.2/bin/idea.sh`

This sentence contains three commands: first, set environment `XMODIFIERS` to null; second,  set `$ JAVA_HOME` to 1.7; third, execute the `bash xx/xxx/idea.sh`.

Setting environment `XMODIFIERS` to null is to avoid some strange input method.

Setting `$JAVA_HOME` to 1.7 in this sentence is very helpful, since it only set the scope of java version only in this process. So I can in the same time building my previous project with java 6 and developing my new project with java 7.

P.S. What you need to do is to change the .desktop file under (in normal case) `/usr/share/applications/xxx.desktop`. sudo, wq, and that’s it. If it doesn’t work, log out and log in again. This might be very important.

2, You can change the jre by doing `export`, and this change just remains in this terminal. So theoretically you can have multiple terminals, each of which has different jre. It's really important to `source` after `export`.