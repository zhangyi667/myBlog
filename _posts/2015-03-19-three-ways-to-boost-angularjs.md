---
layout: post
title:  "Three ways to boost AngularJs"
date:   2015-03-19 18:30:46
categories: work
shortcut: ....
---

1. ####1, Automatically initialization

Angular listens to the `"DOMContentLoaded event"`(which is fired when the document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading). When this event fires, Angular looks for the ng-app directive which designates your application root.

	 <html ng-app='MyModule'>
	 	...
	 </html>

 If angular found this directive, it will:

 1,	load the module associated with the directive.

 2,	create the application injector.

 3,	compile the DOM treating the ng-app directive as the root of the compilation. This allows you to tell it to treat only a portion of the DOM as an Angular application.



2. ####2, Manuallly start:

If you do not add an "ng-app" element, or you need to have more control over the initialization process, what you can do is:

1, in html file, declare controller.

2, in js file, create module of the app and bind the controller.

3, call `bootstrap()` method and pass the module as a param into it.

```javascript
	angular.element(document).ready(funciton(){
	
	angular.bootstrap(document,['MyModule']);

	});
```

3. ####3, Combination:

In one .html file there can be multiple "ng-app" elements, as long as they are not nested angularjs would find the first ng-app and start the corresponding module, as for the others you should bootstrap it using the 2nd way.
This is not common. Do Not bind multiple modules in one page

Details of how the angular bootstrap:

 logic written in angular.js: 

 The js file is a big self execution anonymous function, which creates most of the functions and in the end calls some important functions to bootstrap:

```javascript
 	bindJQuery();

 	publishExternalAPI(angular);

  	jqLite(document).ready(function() {
    	angularInit(document, bootstrap);
  	});

```

The `bindJquery()` method binds to jquery if present, else binds to JQLite, the lite version of Jquery, so that angular js can use the elementory functions of jquery.

The `publishExternalAPI()` does such jobs:

1. 1,Initialize angularModule, which is basically an injector. Until in this step, the angular registers its module
2. 2,Inject default providers
3. 3,Inject default directives

