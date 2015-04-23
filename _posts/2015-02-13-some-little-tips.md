---
layout: post
title:  "Some Little Tips"
date:   2015-02-13 18:30:46
categories: work
---

####1.About how to remove a foreign key in a table(MySQL):

1. a, `find the constraint: show create table TABLE_NAME;` From this request you can see the CONSTRAINt of the foreign key, it's a sequence like "FK84229236324DD57" 

2. b, `alter table user_radius_attribute drop foreign key FK84229236324DD57` this command removes the foreign key

3. c, if removing a primary key, the command is `alter table user_radius_attribute drop key FK84229236324DD57`


####2. ExtJs Array different method comparing

Extjs has a few methods to loop an Array, which are quite similar yet still have differences.

`forEach()`: loop the whole elements of the array, just like for-loop

`every()`: loop the whole elements of the array, but stops looping the first time the iterator returns false or something falsy

`some()`: loops and stops looping the first time the iterator returns true or something truthy

`filter()`: creates a new array including elements where the filter function returns true and omitting the ones where it returns false

`map()`: creates a new array from the values returned by the iterator function

`reduce()`: builds up a value by repeated calling the iterator, passing in previous values; see the spec for the details; useful for summing the contents of an array and many other things

`reduceRight()`: like reduce, but works in descending rather than ascending order

4. d, The way grails to avoid the xss attack:
{% highlight javascript%}
	<g:encodeAs codec="HTML"> 
	 ${a.token.description}
	</g:encodeAs>
{%endhighlight%}

encode the content as html so that the browser will render it instead of judging it as javascript.

5. e, `<meta content="width=device-width, initial-scale=1.0" name="viewport"/>`
This sentence in html file tells browser to adjust width just to comply with the screen width.