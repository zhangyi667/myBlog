---
layout: post
title:  "See the sacrifices angular has done to minification."
date:   2015-03-30 18:30:46
categories: work
shortcut: Taking the first sight of angularJs, you might think ....
---
Taking the first sight of angularJs, you might think it is strange to declare a controller in the way, which in the official tutorial is like:
{{% highlight javascript %}}
	phonecatApp.controller('PhoneListCtrl', ['$scope', '$http',
	function($scope, $http) {
		// here goes your code
	}]);

{{% endhighlight %}}

Why do I have to declare the params twice? And if you have a test, in fact, it is the same with the following code:
{{% highlight javascript %}}
	function PhoneListCtrl($scope, $http) {
		//here goes your code
	}
	PhoneListCtrl.$inject = ['$scope', '$http'];
	phonecatApp.controller('PhoneListCtrl', PhoneListCtrl);

{{% endhighlight %}}

The key is the `$inject`. What is it?

Well, the reason that `$inject`exists is to avoid problems when doing minification. As quoted in the [official tutorial][tutorial]:

`Since Angular infers the controller's dependencies from the names of arguments to the controller's constructor function, if you were to minify the JavaScript code for PhoneListCtrl controller, all of its function arguments would be minified as well, and the dependency injector would not be able to identify services correctly.`

That is to say, without the declaration of $inject, “$scope” and “$http” would be missing after minification. So you have to manually inject them by either way above. If you don't, after minification, the browser drops all the params.

[tutorial]: https://docs.angularjs.org/tutorial/step_05#a-note-on-minification