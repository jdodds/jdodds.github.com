---
layout: post
title: "The Difference Between My Custom Linux Project and Existing Tools"
date: 2012-08-27 20:14
comments: true
categories: linux-customization
---

From a small bit of discussion on
[Hacker News](http://news.ycombinator.com/item?id=4440894) and
[reddit](http://www.reddit.com/r/linux/comments/yx86b/im_trying_to_make_it_easy_for_people_to_generate/),
I thought it would be a good idea to extrapolate a bit on what the differences
are between what I'm trying to do with my project for
[generating custom linux images](http://www.indiegogo.com/customized-linux?a=1092744)
and existing tools like [SUSE Studio](http://susestudio.com/) and
[woof](http://puppylinux.org/wikka/woof). In short, what I'm trying to build has
a very focused scope, and aims to provide a service to people who may not even
know basic things about Linux.

The project I'm trying to launch has a much more limited scope than a tool like
SUSE Studio -- I'm trying to focus on making it easy for individual users to
have access to the setup they use on a daily basis, on their personal
machines. SUSE Studio is a well-polished tool, but puts a lot of stuff in your
face by default that the type of user I'm targeting might not know the first
thing about.

The focus, interface-wise, in my tool is going to be on applications and user
data. While I do plan on allowing people to tweak things at as fine-grained of a
level as they want to, I really want to avoid overwhelming less-technical users
with the potential complexity of everything that can go into a distro.

Tools like woof and other existing scripts for generating ISOs require a fair
amount of knowledge from the end-user. I want to provide something that allows
people to spec everything out if they want to, but will by default provide them
with the software they use in a live environment, and after a fresh install,
even if they are close to entirely ignorant of what's going on. This includes
doing things like providing a sane partitioning scheme by default and making sure
that the live and installed environments are the same in the ways that matter.

I'm trying to build something that's usable by people who aren't even close to
being Linux gurus. Ideally, I'm trying to build something that's usable by
people who don't know the difference between a web-browser and a bootloader.

