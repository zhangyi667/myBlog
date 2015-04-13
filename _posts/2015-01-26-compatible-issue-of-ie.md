---
layout: post
title:  "Compatible Issue of IE on ExtJs"
date:   2015-01-26 08:30:46
categories: work
---
Here is a compatible issue of IE on ExtJs projects that I've found during work.

Lately I just released an update version of the project and here came the issue: A button didn't work on IE browser. When user clicked the button, it was expected to open a new window, however, it did not happen. An error was threw by IE. The issue was "Ajax request problem: error 80020101". I didn't know what that was, so I googled it, which gave me answer on [stackoverflow.com][source]: 

`All the error 80020101 means is that there was an error, of some sort, while evaluating JavaScript. If you load that JavaScript via Ajax, the evaluation process is particularly strict.`

I did use Ajax when clicking the button, so I just deleted all the Ajax Requests, but still not work. Then I realized that although I did call the Ajax Request, the function of the button was first to create a new window, then load the content using the elements from the result of the Ajax Request. So if it were the Ajax's fault, I should have a window that had nothing on it. But I even did not have the window. 

So probably it was not/not only the Ajax. So I followed the traceback of IE, which pointed out where it all went wrong. It was this sentence: "params.return.push(blahblahblah)". `params` was an Object that sent through the Ajax Request as Param, and `return` was a hashMap. If this was wrong, that means the "get()" method of the Object is not supported in IE. So I tried to change it to `params["return"].push(blahblah)`  and it worked. 

Stupid IE :(

ExtJs version: 4.1

IE version: 7,8,9,10,11 has been found to have such issue, 12 and above has not been tested.

[source]:http://stackoverflow.com/questions/10903989/could-not-complete-the-operation-due-to-error-80020101-ie