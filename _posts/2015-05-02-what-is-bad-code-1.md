---
layout: post
title:  "Some words about bad code (Part 1)"
date:   2015-05-02 18:30:46
categories: work
shortcut: What is bad code? Have you seen bad codes?
---
Recently I have reviewed and refactored some code since one of my colleague left. Shity codes. I’ve been in touch of these shity codes for weeks. And in this article I want to talk something about bad code, and why bad code happens.


As a programmer I believe nobody wants to write shity code. We want to type perfect, elegant code as art. However, it takes time. In most cases your boss don’t give you enough time. It happens, and in most cases you tell yourself this is just for the moment, and you’ll refactor it in future, which you know it will never happen. 


I’ve been through this. My boss gave me three weeks for a two-month project. I complained that I couldn’t do that, yet my boss promised me a week holiday and I took the deal. That three weeks was a disaster but I managed it. It worked perfectly. I expected my boss to say something good to me, but all I heard is “You were doing it too slow. You should learn from Tom”. Tom was that colleague. 


Then I understood what happened when he left and I had to review his code. His code is only readable to him, and since he had maintained that project for years, it was perfect. Nothing happened. In my boss’ side, he had done the project on time, which passed test, and run smoothly after it was published. So what do you want? 


What I want is very simple. Write the fucking good code.


I have to admit that in recent days more and more “programmers” joined this field. They have no idea how the framework works, why using this framework, or how to raise efficiency. For instance, if you were a Java back end developer, ask yourself why your project used Spring MVC with Redis. Why not Memory cache, why not strut 2, why not Grails. But since the framework has involved day by day and it handles most of functions, all you need to do is to write some easy code and it works. Slow but works.


But bad code is bad code. You can’t change that. 


There are some kinds of bad code. If you have written code like this, you should review yourself as code.


1, "What the hell do you want to do?"

{{% highlight javascript %}}
	public void sendEmail(String message, String email) {
		for (int i = 0; i < 100; i++) {
			if (!send(message, email)) {
				i++;
			} else {
				return;
			}
		}
	}
{{% endhighlight %}}

For who have written this, please change another job.

2, "Can you do it more complex?"

This kind of code runs well, but it will take you days to figure out how it works.

{{% highlight javascript %}}
	public boolean fileReadable(String path, String serverUrl) {
		if (path == null) return false;
		FileConfigure conf;
		if (serverurl.startsWith('xxx') || serverUrl.indexOf("backup") != -1) {
			conf = new ServerAConfigure();
		} else {
			conf = new ServerConfigure();
		}
		if (path.charAt(path.length() - 1) == '/' || path.charAt(path.length() - 1) == ';' || path.charAt(path.length() - 1) == '..') {
			path = path.substring(0, path.length() - 1);
		}
		int ncfs = conf.findFile(path);
		if (ncfs < 1) return false;
		else if (ncfs == 1) {
			List<Server> servers = conf.getServers();
			for (Server server : servers) {
				server.backupFile(path);
			}
		}
		return true;
	}
{{% endhighlight %}}

What's wrong with this code? First, as a method, it should be simple, only do one thing at a time. Look at this method. It verifies the path, gets conf according to serverUrl, and does backup before returning results. And believe it or not, the variable "ncfs" means "numbers of copied files in all servers". Some programmers love writing shortcuts, even it affects readability.

Is this really for human?

Just a few minutes work it could be more readable: 

{{% highlight javascript %}}
	public boolean fileReadable(String path, String serverUrl) {
		if (path == null) return false;
		FileConfigure conf = getConfFromServerURL();
		String newPath = verifyPath(path);
		int ncfs = conf.findFile(path);
		if (ncfs < 1) return false;
		else if (ncfs == 1) {
			otherASyncThreadDoBackupFiles(path);
		}
		return true;
	}

	private FileConfigure getConfFromServerURL(String serverUrl) {
		FileConfigure conf;
		if (serverurl.startsWith('xxx') || serverUrl.indexOf("backup") != -1) {
			conf = new ServerAConfigure();
		} else {
			conf = new ServerConfigure();
		}
		return conf;
	}

	private String verifyPath(String path) {
		if (path.charAt(path.length() - 1) == '/' || path.charAt(path.length() - 1) == ';' || path.charAt(path.length() - 1) == '..') {
			return path.substring(0, path.length() - 1);
		}
		return path;
	}

{{% endhighlight %}}

Yet it is still not good code, which I'll explain in next article.