---
layout: post
title: "Devops on an iPad"
date: 2013-07-10 00:36
comments: true
categories: 
---

When I started software development I worked pretty much exclusively on my
personal notebook, in my lap. I ended up graduating to a workstation setup like
this by 2011.

IMAGE

That's a [Henge Dock](http://www.hengedocks.com/) with a pretty heavily upgraded
13" Macbook Pro, and a [Unicomp Classic](http://pckeyboard.com/page/UKBD/UB40P4A).
What you can't see is the big server in the next room with kernel-based virtual
machines running on it. 

This setup gave me a ton of flexibility and the freedom
to work on static websites, web applications, and devops configurations with
little to no interruption or reconfiguration. If you like having a conventional
desktop workstation, I can highly recommend every bit of this setup. 

When I started working for Sudo in early 2013, I shifted from a role of combined
operations and development, to full-time operations engineering. This meant that
most of my time was spent SSH\'d into other servers, and what time wasn't spend
there, was spent in the Amazon Web Services console.

Also I had to go to an office. On this.

IMAGE

Taking in a laptop every day with the possibility of accidents and rain wasn't
appealing to me. Add to that the weight of a laptop, and I wasn't looking
forward to my commute. 

I've never really gotten the point of tablets. That's not to say I think they're
bad, but I never had any urge to buy one. Faced with my new commute and a new
job, they were suddenly starting to look appealing. 

I was already spending all of my time in a single application anyway. If all I
needed was Terminal and a bit of Chrome on my laptop, why should it be any
different on the iPad? Full screen terminals are a perfect work environment, and
that translates to the tablet ecosystem better than a whole lot of other tasks
that have ended up on there. 

As long as I could get a half-decent terminal emulator and SSH client on an 
iPad, I could hook it up an an EC2 instance inside Sudo's VPC and be right back
to work. 

NOTE: I really wanted to make this work on a Microsoft Surface RT, but the
software just wasn't ready. 

Now I bike to work with an iPad on the back of my bike.

IMAGE

It's way lighter, way harder to break, and a lot less to lose if it gets stolen.
Coupled with the fantastic [Moshi Versacover](http://www.moshimonde.com/product/iglaze-with-versacover.aspx), 
a bluetooth keyboard, and [iSSH](http://www.zinger-soft.com/iSSH_features.html)
you actually end up with a pretty capable development machine.

Unless you need the AWS Console. 

If you need the AWS Web Console You're going to have a terrible time with this
setup. I had to entirely abandon the web interface for AWS when working from my
iPad. It's sluggish at best, dangerous at worst. Most of the time it just takes
forever to load anything, and once it has, good luck interacting with it.
Often, Safari (or Chrome, or Opera) will simply crash when trying to load a new
interface component. If you're really unlucky your impatient taps will finally
get registeres after two or three minutes of waiting, only to incorrectly
configure a new load balancer of Route53 record. 

When it comes to AWS, use the [CLI tools](https://aws.amazon.com/cli/), or one
of the SDKs. I've used both [Ruby's aws-sdk](https://aws.amazon.com/sdkforruby/)
and [Python's boto](https://aws.amazon.com/sdkforpython/) and both are
fantastic.

The bottom line is that the iPad can be a great alternative to a conventional
desktop workstation, but it takes some serious getting used to, and has some
real limitations.

More
-----
If you're interested in the idea, I really recommend checking out these links. 

- [I swapped my MacBook for an iPad+Linode](http://yieldthought.com/post/12239282034/swapped-my-macbook-for-an-ipad)
- [iPad + Linode, 1 Year Later](http://yieldthought.com/post/31857050698/ipad-linode-1-year-later)
- [Binary - Development Environment for iPad](http://thebinaryapp.com/)
