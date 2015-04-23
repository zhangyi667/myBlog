---
layout: post
title:  "Enum Class Leads Grails Crush"
date:   2014-10-20 18:30:46
categories: work
shortcut: I was having a vacation in Edinburgh for couple of days. When I came back...
---
I was having a vacation in Edinburgh for couple of days. When I came back, some code of the server has been updated by my colleagues. 

As I pulled the code and tried to start server, it went crush. This happens a lot when new code has been pulled from git, and I thought I had gotten used to it. But I was wrong. After checking the log, I found the issue was caused by package dependency, which was solved as soon as I re-compiled the core libs we used. I thought this was over, but it just began.

The server did start, and I opened the web-application and tried to log in. This time, an exception “No enum const xxx.EnumClassA” came out. This is strange since Enum classes are loaded when server started. If server cannot find any enum classes or went wrong when compiling the classes, exceptions would be threw in the first time.

So I set breakpoint to see what happened. Just to find that, issue happened when the EnumClassA called the method 'valueOf()', which requested params. And not surprisingly, the params was empty. Reason's been found. It's not "No enum class something", it was "Encountered issues when get the value of the enum class".

{% highlight Java %}
	public static <T extends Enum<T>> T valueOf(Class<T> enumType, String name) {
	        T result = enumType.enumConstantDirectory().get(name);
	        if (result != null)
	            return result;
	        if (name == null)
	            throw new NullPointerException("Name is null");
	        throw new IllegalArgumentException(
	            "No enum const " + enumType +"." + name);
	    }
{% endhighlight %}

The code explain everything. It threw “No enum const” message as long as the result is null. What a misleading exception.

So I check why the params is empty. The value of this param is from another API, so I step into that API, and I found that the attribute of that Object is null, which was not expected to happen since every attribute has a default value.

So I looked into the wiki, where the detail info of this API can be found, and it turned out to be created during my vacation... Finally I’ve found the answer. Some one updated the database, add some new shining attributes without telling me, thus the database that I used was too old which did not contain the attribute, thus when calling the database, such attributes is empyt. This took me a whole morning :(
