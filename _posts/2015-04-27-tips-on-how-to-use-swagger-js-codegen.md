---
layout: post
title:  "Tips on grunt-swagger-js-codegen"
date:   2015-04-27 18:30:46
categories: work
shortcut: This article explains how to use grunt-swagger-js-codegen..
---
Swagger is a fantastic tool to help develope API. It automatically generate a page to show the response. Moreover, it generates the class object for different lunguages so when you call the API you can use the Object instead of calling it yourself. This is great. Swagger does not have a Javascript codegen, but with a third-party plugin we can do that. It is called ["swagger-js-codegen"][codegen]. It is brillient, however, with the little help of its document, you would easily get lost. In this article, I will tell you how to do that.

In the beginning what you should have are:

`grunt, npm`

Then what you should do is to install swagger-js-codegen:

	`$npm install swagger-js-codegen`
	`$npm install grunt-swagger-js-codegen` // this is important and didn’t mentioned in the github!

In your gruntfiles.js,

1, add this sentence `grunt.loadNpmTasks('grunt-swagger-js-codegen')`;

2, modify the initconfig:
{{% highlight javascript %}}
	 grunt.initConfig({
	        'swagger-js-codegen': {
	            queries: {
	                options: {
	                    apis: [
	                        {
	                        swagger: “path-to-json-file/user.json”',
	                        moduleName: ‘yourApp',
	                        className: ' UserService',
	                        fileName: 'UserClass.js',
	                        angularjs: true

	                        }
	                    ],
	                    dest: “path/to/the/dest”
	                },
	                dist: {
	                }
	            }
	        }
	    });
{{% endhighlight %}}

And in terminal, run `grunt swagger-js-codegen`.

What has been generated is UserClass.js, inside which is the Object:
{{%  highlight javascript%}}
	angular.module(‘yourApp',[]) 
	.factory(‘UserService',[...]){
	…
	}
{{% endhighlight %}}
 

Finally, if you want to use it, like in your controller, you have to include and new a class before calling any API.

{{% highlight javascript %}}
	var user = new UserService();
	...
{{endhighlight}}

[codegen]:https://github.com/wcandillon/swagger-js-codegen
