---
layout: post
title:  "How to Join table in grails"
date:   2014-04-08 08:30:46
categories: work
---
In Grails, building a many-to-many relationship between two objects is simple, for exmaple, if we are binding objects Person and Job, what we do is to:

in Person.groovy:
{% highlight groovy %}
static hasMany = [jobs:Job, .....]
{% endhighlight %}
in Job.groovy:
{% highlight Java %}
static hasMany = [persons: Person, ....]
static belongsTo = [Person, .....]
{% endhighlight %}

And we bind them together, in the mean time job belongs to person. And Grails creates 3 tables in the database: person, job, person_job. In table person, there is one attribute called "job_id". In table "person_job", there are 3 attributes: person_id, id and job_id.

But we all know, that oracle is very sensitive to the length of the attribute name, which for now is built by Grails. What if one day I have to bind together two Objects, respectively "whereIsEveryBodyGoingTo" and "whoIsNextForSomeCoffee"? Grails will automatically create a table "where_is_every_body_going_to_who_is_next_for_some_coffee".....which is totally of out the range of the name limit.

So can we name the table ourselves?

Of course you can. To do the mapping you need to do it like this:
in Person.groovy:
{% highlight groovy %}
static hasMany = [jobs:Job, .....]
static mapping = {
    ....
    jobs joinTable : "p_j"
    jobs column : "person_jobs_id"
}
{% endhighlight %}
in Job.groovy:
{% highlight groovy %}
static hasMany = [persons: Person, ....]
static belongsTo = [Person, .....]
static mapping = {
    persons joinTable: "p_j"
    persons column: "job_id"
}
{% endhighlight %}

Thus "p_j" is the table name that grails builds in database. Keep in mind that you have to write it twice, and do not get the name wrong.