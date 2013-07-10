---
layout: post
title: "High availability postgres on AWS: The Problem"
date: 2013-06-30 
comments: true
categories: 
---
At [Sudo](http://gosudo.com) we use Postgres for our database backend. Postgres 
is a fantastic database that provides a lot of functionality and stability over 
MySQL (and if you really want to use MySQL, you should use 
[MariaDB](http://mariadb.org/) instead). However, there is one big caveat: 
AWS doesn't support Postgres for RDS yet. This means that if you want high 
availability and scalability with Postgres, you're going to have to implement 
your own solution. 

Yeah, things like [Cloud Postgres](http://www.cloudpostgres.com/) exist, but 
they're nowhere near the power of RDS (that's not to bash on Cloud Postgres, 
which is a really cool platform). So if you can use MySQL, Oracle or MSSQL, 
I'd recommend doing that, and saying goodbye to  Postgres until Amazon 
integrates it into RDS. This will give you easy access to to performance and
reliability. You really can't beat it. 

{% img center /images/yourproblem.png 350 350 'image' 'images' %}


-> *Your Problem* <-


{% img center /images/notyourproblem.png 350 350 'image' 'images' %}


-> *Not Your Problem* <-

However, if you're like Sudo, you don't really have that choice. We use location
data extensively for to help users find deals near them, and that means we need 
a location-aware DB. Check out 
[these tables](http://www.bostongis.com/PrinterFriendly.aspx?content_name=sqlserver2008_postgis_mysql_compare)
for a good comparison of the available location-aware databases. Then compare that with 
[Django's ORM compatability](https://docs.djangoproject.com/en/dev/ref/databases/#using-a-3rd-party-database-backend)
and compare all of that to [GeoDjango's compatible database
backends](https://docs.djangoproject.com/en/dev/ref/contrib/gis/db-api/)

The result is that we're pretty much stuck with Postgres, and that means
we have toroll our own database infrastructure.

## What Do We Want?

So our requirements are something like this (and in this order)

1. Support location data
2. Support Django
3. Provide high availability
4. Provide scaling capacity

Just by picking Postgres+PostGIS we get #1 an #2, literally for free. If you're
curious how easy it is to set up a location aware Postgres database with Django,
well, [It's not that easy, but it's not that
hard either](https://docs.djangoproject.com/en/dev/ref/contrib/gis/install/postgis/)

Getting the last two problems solved is a lot harder than the first two, though,
and it's probably worthwhile to at least examine the basic reasons you might
want each. 

If you're comfortable with operations engineering, you can probably skip this 
next bit safely. 


### High Availability


Any sysadmin should crave uptime. Ideally no part of your
infrastructure should be a single point of failure for your application. This
means clustering, failover, replication, and more. The goal should be not just 
to keep your application from going offline, but to keep you and your team 
from having to rescue or rebuild it by hand. Ideally, a database server could 
go down and the rest of your cluster could recover without going offline. Better
yet, a replacement server could be automatically provisioned, and joined to the 
cluster to replace the broken node. If you can set this up across all 
availability zones in an AWS region, and you get a database cluster that's very 
hard to take down without an act of God (or a very mistaken sysadmin).

The fast pace of modern development, taken together with the complicated systems
that they leverage means that things will inevitably break. High availability
architecture ensures that a single server failing doesn't cause any serious
damage or downtime to your application.

This is equally good for developers and users. For operations engineers, this 
means no 3am calls from Nagios requiring you to manually repair a database.

For users, it means the application is always ready for them whenever they want
it. 


{% img center /images/reddit.gif 350 350 'image' 'images' %}

-> Avoid this <-


### High Scalability

While Sudo doesn't need the sort of performance that apps like Instagram or
Pinterest use AWS to achieve, we don't want to be locked into a low-performance
solution for our database, just because it fit our needs early on. 

The goal here is to build a database layer that can increase and decrease in
performance in order to best fit our needs at any given time. This
means that not only should we have the ability to easily scale our database 
to respond to increases in user demand, but we should also be able to decrease
during lulls in order to minimize cost.

What's Next?
------------

The goal is to use Chef, and some combination of Amazon tools in order to
provide this scalable, resiliant location-aware database architecture. If you're
interested in the options, check out part two for a few possible solutions to
this problem.
