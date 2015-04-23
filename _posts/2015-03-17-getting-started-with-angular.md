---
layout: post
title:  "Getting started with AngularJs"
date:   2015-03-17 18:30:46
categories: work
shortcut: It all begins with module...
---
It all begins with module. 

For example, we are building a project `“bookstore”`.

The first file we’re gonna see is the `“app.js”`, whose name is convention. Of course you can name it whatever you like. In this file, we do module injecting, controller injecting, config injecting, etc. Let’s say we wanna register a module ‘bookStoreApp’.

The way to register a module in a project is like:

{{% highlight javascript %}}

  //....
  angular.module(‘someModule’,[‘service1’, ‘service2’, ‘service3’]);
  //...
  //‘ServiceX’ is the service that this module depends on. When angular is doing the compilation, it inject those services into the module so this module can call them.

  //In this case:

  var bookStoreApp = angular.module(‘bookStoreApp’, [
    'ngRoute',
    'bookStoreControllers',
    'bookStoreFilters',
    'bookStoreServices'
  ]);

  //Then, define the route strategy in config:

  bookStoreApp.config(['$routeProvider',
    function($routeProvider) {
      $routeProvider
  .when('/books/:bookId', {
          templateUrl: 'partials/book-detail.html',
          controller: 'bookDetailCtrl'
        })
  .otherwise({
          redirectTo: '/books/1'
        });
    }]
  );
{{% endhighlight %}}

Note: Variables with `$` as prefix are angularJs built-in services. To know how they can be used, see the API document. The configuration defines a simple and basic route strategy that shows different html files with different controllers. So the next step is to create controllers.

In another file, in this case controller.js, let's declare a controller.

{{% highlight javascript %}}

  var bookStoreCtrl = angular.module('bookStoreControllers', [])

  //this declares that the module is “bookstoreControllers”, which must be the same to the one declared in app.js

  bookStoreCtrl.controller('BookDetailCtrl', ['$scope', '$routeParams', 'Book',
    function($scope, $routeParams, book) {
      $scope.book = {name:”some name”, author: “somebody”};
  }]);

{{% endhighlight %}}

In the config it wraps `“BookDetailCtrl”` and `“book-detail.html”`, so in book-detail.html we can use whatever has been defined in BookDetailCtrl.

For example, if the html goes like: 

...

	<h1>{{book.name}}</h1>

  <p>{{book.author}}</p>

...

Then what the browser would get is :

...

	<h1>some name</h1>

  <p>somebody</p>

...

And so far that’s what you need to take care when bootstrapping from the angularJs.

P.S. little tips

1. 1,The syntax for using filters in Angular templates is as follows: {{ expression | filter }}

2. 2,The RESTful functionality is provided by Angular in the ngResource module.

3. 3,Angular normalizes an element's tag and attribute name to determine which elements match which directives. So when creating directives remember the name spells are different. For example, if a directive name is “myDir” when declaring, in .html files it might be named “my-dir”. Angular has its own strategy to normalize the name: [see here][docs]. 

4. 4,For AngularJS, "compilation" means attaching event listeners to the HTML to make it interactive.

5. 5,Link function is used to bind event on element, or bind scope, or get attrs.

6. 6,The compile() method is also in return closure. The return closure of compile will overwrite link function, because the return closure of compile() is link().

7. 7,You cannot use scope in compile process, because the scope has not been binded with elements until the link process 


[docs]:https://docs.angularjs.org/guide/directive#normalization