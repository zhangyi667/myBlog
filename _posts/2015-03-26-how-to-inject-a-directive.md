---
layout: post
title:  "How to inject a directive after Angular has boosted"
date:   2015-03-26 18:30:46
categories: work
shortcut: This is a very dump question but I have learned something from it...
---
This is a very dump question but I have learned something from it....

Currently I’m using AngularJs to implement a draggable field(like, an image). I used a directive to implement the function. It works when I load it before angular boosted. But if I tried to load the directive when the page has been loaded, the draggable field cannot be dragged. In other word, the directive has not been injected after Angular has boosted. 

But what I expected was that Angular could self-inject the directive so all I need to do is define it....

In fact, AngularJs has an injector mechanism which provides you such function to solve my problem.
To inject an element with some directive all you need to do is:
{{% highlight javascript %}}
	//find the injector

	var $injector = angular.element(document.querySelector('#container')).injector();

	//element is the target dom that you want to inject to

    var element = angular.element(document.querySelector('[name="Dusty"]'));
{{% endhighlight %}}

{{% highlight javascript %}}

	//let $compile do the inject thing    

    $injector.invoke(function ($compile) {    
        var scope = element.scope();
        $compile(element)(scope);
    });

{{% endhighlight %}}

And that’s how it works.
To see a complete example [poke me][wxample]
[example]: http://jsfiddle.net/6n7xk/