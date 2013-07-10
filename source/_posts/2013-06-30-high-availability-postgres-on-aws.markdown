---
layout: post
title: "High availability postgres on AWS"
date: 2013-06-30 13:01
comments: true
categories: [Django, GIS, location, database, scaling]
---
At [Sudo](http://gosudo.com) we use Postgres for our database backend. Postgres 
is a fantastic database that provides a lot of functionality and stability over 
MySQL (and if you really want to use MySQL, you should use 
[MariaDB](http://mariadb.org/) instead). However, there is one big caveat: 
AWS doesn't support Postgres for RDS yet. This means that if you want high 
availibility and scalability, you're going to have to create your own solution. 
Yeah, things like [Cloud Postgres](http://www.cloudpostgres.com/) exist, but 
they're nowhere near the power of RDS (that's not to bash on Cloud Postgres, 
which is a really cool platform). So if you can use MySQL, Oracle or MSSQL, 
I'd recommend doing that, and saying goodbye to  Postgres until Amazon 
integrates it into RDS. This will give you easy access to to performance and
reliability. You really can't beat it. 

However, if you're like Sudo, you don't really have that choice. We use location
data extensively for to help users find deals near them, and that means we need 
a location-aware DB. Check out 
[these tables](http://www.bostongis.com/PrinterFriendly.aspx?content_name=sqlserver2008_postgis_mysql_compare)
for a good comparion of the available location-aware databases. Then compare that with 
[Django's ORM compatability](https://docs.djangoproject.com/en/dev/ref/databases/#using-a-3rd-party-database-backend)
and compare all of that to GeoDjango's compatible database backends 

**TL;DR** Sometimes you just gotta roll your own infrastructure.

## Planning It

So our requirements are something like this

1. Support location data
2. Support Django
3. Provide high availability
4. Provide scaling capacity

Just by picking Postgres+PostGIS we get #1 an #2, literally for free. If you're
curious how easy it is to set up a location aware Postgres database with Django,
my chef recipe says the server config takes about 3 steps. Numbers 3 and 4 are a
bit trickier though. 

### High Availability


Any sysadmin should crave uptime. Ideally no part of your
infrastructure should be a single point of failure for your application. This
means clustering, failover, replication, and more. The goal should be not just 
to keep your application from going offline, but to keep you and your team 
from having to rescue or rebuild it by hand. Ideally, a database server could be
go down and the rest of your cluster could recover without going offline, and
a replacement server could be provisioned, and joined to the cluster. This
should all happen without you having to lift a finger. Build a system like this
across all availability zones in an AWS region, and you get a database cluster
that very hard to take down without an act of God (or a very mistaken sysadmin).

### High Performance

The advantage of this system is that not only do you get high availaility, 

## Rolling It
