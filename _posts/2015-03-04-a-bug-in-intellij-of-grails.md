---
layout: post
title:  "A bug with IntelliJ of Grails"
date:   2015-03-04 18:30:46
categories: work
shortcut: Grails 2.3.x and Intellij 12.x and above.....
---
When doing grails project with version 2.3.x and above in Intellij with version 12.x and above, there will be likely a bug that debugging in intellij doesn’t work. This relates to grails fork execution. To avoid that, in BuildConfig file, either adding :
{% highlight Java%}
	grails.project.fork = [
	    test: false,
	    run: false
	]
{% endhighlight%}
or, if using intellij 13 and above, comment the lines started with `run` and `test`.