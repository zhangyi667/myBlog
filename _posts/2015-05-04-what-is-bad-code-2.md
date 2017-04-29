---
layout: post
title:  "Some words about bad code (Part 2)"
date:   2015-05-04 18:30:46
categories: work
shortcut: What is bad code? Have you seen bad codes?
---
3, “I’m not able to handle this project”

This is high-level bad code, which is written by some junior programmer. After doing some projects, you can write high quality methods. However, a project is too big for you to handle. In class 101, your teacher told you that good code is “Low cohesion and high coupling”. Now you get to know it. You managed the project, yet the way you did this was way too unprofessional. 

- Same bunch of code is copied and pasted everywhere.
- Essential path variables is defined in inner method rather than conf files.
- Over 100 lines in one method.

What’s gonna happen in such code is, it is too complex and difficult to change, even one single line might cause crush. That’s why when adding features of the project, they intend to modify the code and make it more complex. In a couple of commits, nobody can figure out the logic.

4, “I think too little before coding”

When your boss told you to develop a function to read content from file and return it, what you gonna do?

- “Think before coding”

I’ve seen this a lot when my boss told me to do some feature and change demand after a few days. Shit happens and happens a lot. So what’s important is to leave enough space in case demand changes. This requires experience.

So how to code that file-read function?

{{% highlight java %}}
	public String readFile(String path) {
		File file = new File(path);
	}
{{% endhighlight %}}

Is that alright? 

- What does path mean? Relative path or absolute path? Will it be a url?
- Is it only outputing a String?
- What is the character encoding? UTF-8? Will it change?

Summary: It takes a lot to be a good programmer. It takes a lot to write high-quality code. If you think you have done it, review your code in such steps: Function, Performance, Readable, Testable, Extention. 

I've seen some really ridiculous code, written by programmers who think they are gonna maintain it forever, which will leave a lot of trouble for those who take over the job. That's what I complain.

I know it is hard to balance time and quality. But, I say but, if you choose to sacrifice quality to save time, you will end up spending much more time to fix bugs.

You might argue, "I really don't have that time. And if this is not handy, we just refactor and everything's gonna be fine." 

Remember me: Refactor can't fix everything.

On developer side, refactor is the most complex thing to do. I've been into a few refactors, and all of them are disasters. All good programmers took part in, and all developing works pasued. I heard tons of complation everyday, and still when it published, the refactor errolled more bugs. To do refactor, you need to understand old code, dissect old code, then write new code. As I said, bad code happened.

On project manger side, what makes refactor disgusting is, it is almost impossible to evaluate time. You know last time it took one hour to refactor 1000 lines of code, but you can't tell this time how long it takes to refactor 5000 lines of code. 3 days, 5 days, or even a week. Moreover, the benefit of refactor is nearly zero at the moment.

On your boss side, it takes that much time, that many developers, to do something that he does not understand but looks like it doesn't change anything, except it enrolls more bugs. It is very hard for him to tell it is NOT a waste of time.

To be continue....