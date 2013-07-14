---
layout: post
title: "High availability postgres on AWS Pt 2: Options"
date: 2013-07-13 19:53
comments: true
categories: 
---

In [Part 1](/blog/2013/06/30/high-availability-postgres-on-aws/), we talked
about the need for high availability, and the trouble with getting there using
Postgres on AWS. In part 2, we're going to examine two possible options for
gaining extra availability and scalibility.

Proper warning: I am **not** a Database Admin. I'm responsible for the design
and implementation of many different kinds of tools for Sudo. My goal is to
build the least complex solution that fully solves our problems, and my choices
reflect that. 

Alright, with that out of the way, let's get right to it.

## Postgres 9.x with Streaming Replication

This is the solution that I know the least about and did the least research
into, but I'm including a mention of it here because I think it may be a very
reliable answer to this problem, if only you have more time and patience than I
do. 

The gist of this configuration is that you run multiple Postgres servers in
[streaming replication mode](https://wiki.postgresql.org/wiki/Streaming_Replication).
This is a configuration option that is built into postgres itself, and requires
no outside software to manage, so far as I can tell. 

The problem is that it's not simple. Not in the least. Obviously nothing is
simple when it comes to database scaling, but this setup looked particularly
intensive. 

I think anyone with the patience to slog through 
[the official documentation](http://www.postgresql.org/docs/9.0/static/high-availability.html)
absolutely ought to. I simply didn't have the time, and I knew simpler options
were out there. Aside from the complexity, my only concerns were that I'd need
to end up using some extra piece of software for load balancing, and that
correctly load balancing reads/writes would end up being even **more**
difficult. 

I was also slightly concerned with how to use Chef to manage such an
infrastructure, since ideally all the nodes are identical, but due to the nature
of standbys and masters and standbys, you end up with a sort of
first-among-equals problem.

## Along Comes Pgpool

Enter [pgpool](http://www.pgpool.net/mediawiki/index.php/Main_Page)
The next two solutions lean heavily on pgpool. Pgpool, with its streaming
replication mode, load balancing, node recovery and other features make it an
excellent alternative to using postgres itself to provide replication. 

Here's a quick rundown of what we get from pgpool

- Load balancing
- Health checks
- Streaming replication
- Online recovery

### Online Recovery

It's worth spending a little time talking about online recovery alone.

What it means is that if one backend database gets out of sync with the others, 
pgpool will notice and attempt to bring the node back into sync with the rest of
the pool. The most obvious use for this feature is to help surviving against 
temporary outages of availability zones without having to re-provision entire 
clusters by hand. 

But the best part of online recovery is that you can add new instances to the
pool dynamically. This means you can add more databases backends as you need
them, and you can take down individual instances to increase their size in AWS
without having to lose database access or manually recover them back into the 
pool once they boot back up. 

### SPOF

The next two solutions are all based on the premise that pgpool will be handling
the majority of the complex backend task of keeping data synchronized and load
balancing reads and writes. A basic configuration looks a little something like
this.

{% img center /images/pgool-basics.png  %}

This configuration provides all of the wonderful benefits of onlne recovery and
load balancing, and it gives us a bit of extra security against outages, but
there's one *big* problem that comes along with this setup. Pgpool now 
represents a SPOF, or single point of failure. If that one pgpool goes down, it 
takes out all database access. In the case of AWS, this means outage of a single
availability zone can take down your whole application.

In Part 3 we'll look into some configurations that can reduce the risk of this
SPOF. 
