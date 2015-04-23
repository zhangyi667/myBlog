---
layout: post
title:  "Learning AngularJs notes"
date:   2015-03-18 18:30:46
categories: work
shortcut: I have been learning AngularJs these days.....
---
I have been learning AngularJs these days and here are some tips that I have learned.
{{% highlight javascript %}}
	//angular Js directive,
	//directive returns a config, of which the common variables are "restrict", "template", "templateUrl","replace", "transclude", "link"

	var myModule  = angular.module("MyModule",[]);

	//here we define a directive "hello"
	myModule.directive("hello", function(){
		return{
			restrict:"AEME",
			template:'<div>Hello</hello>',
			replace: true

		}
	})

{{% endhighlight %}}
`restrict = "AEMC" `(means attribute, element, comment and class respectively) defines how the directive can be used.


angular has a provider $templateCache, which can cache something you want it to cache,
e.g.

{{% highlight javascript %}}
	//when the injector has loaded all the modules
	myModule.run(function($templateCache){
		$templateCache.put("hello.html", '<div>Hello</hello>');
	});


	//when you want to use the cached content, just:
	myModule.directive('hello', function($templateCache){
		return {
			restrict: 'AEMC',
			template: $templateCache.get('hello.html'),
			replace: true

		}
	});


	//how to use attribute 'transclude'
	 
	myModule.directive("hello", function(){
		return{
			restrict:"AEME",
			template:'<div>Hello <div ng-transclude></div></hello>',
			replace: true

		}
	})


	//link is used to do the interaction of directive and controller,
	//such as add event-listening support, and reuse of directive


	//make scope independent:
	return{
		...
		scope:{},
		...
	}
{{% endhighlight %}}