---
layout: post
title: "a more in-depth look at how what I'm trying to do is different"
date: 2012-09-03 09:39
comments: true
categories: linux-customization crowdfunding
---
A few days ago, I posted a link to [reddit](http://www.reddit.com) about the
["Customized Linux" project I'm trying to get funded](http://www.indiegogo.com/customized-linux). It
generated
[quite a bit of discussion](http://www.reddit.com/r/linux/comments/yx86b/im_trying_to_make_it_easy_for_people_to_generate/),
which you are, of course, free to read through if you're so inclined.

One thing that became clear was that I **really** needed to clarify just what
exactly it is that I want this project to be and why the existing tools that let
people customize Linux ISOs don't cut it. I posted
[a short attempt at clarification](http://jdodds.github.com/blog/2012/08/27/the-difference-between-my-custom-linux-project-and-existing-tools/),
but for one it still wasn't clear at the level that it needed to be, and for two
I didn't even really get a very solid phrasing of the idea in my head until
a few hours after I had posted it.

I'm hoping that this post will provide a clear and understandable overview of
what I'd like to accomplish. We'll start by taking a look at what the problems
I'd like to solve are, and then take a look at why existing tools that have
similar functionality aren't solving those problems. We'll wrap up with a bit of
exposition on how we can go about creating good solutions for those problems.

<!-- more -->

## The Problems
One of the biggest mistakes I made when launching the indiegogo campaign for
this project was putting a focus on it being something that will create
customized Linux install and installation images. While it does do that, it's an
implementation detail, and there are plenty of existing tools that do similar
things.

Let's take a look at the problems I'd like to solve. There are a few major
problems, and this project would solve all of them. As far as I know, there
aren't any good existing tools that do so.

### You Should Be Able to Have Your Software and Data Available Anywhere
We live in a pretty amazing time. According to a lot of people, it's the future
-- and it certainly seems that way sometimes. We can communicate with people on
the other side of the globe easily, and there are lots of things that we do
everyday that were only barely imagined by the science fiction authors of the
50s and 60s.

I want to make it so that any computer you use can effectively be **your**
computer with a reboot. 

Just about everyone who uses a computer on a regular basis has some set of
programs that they like to use, and some files that they use often or need to
have on hand often. People often have some configuration set up that they
like. Examples of this are instant messaging programs, Dropbox accounts, desktop
backgrounds, Firefox themes, papers they're working on for school, and so on.

Personally, I've spent quite a bit of time setting up my computer so that I can
be as productive as possible. Unfortunately this means that working on other
people's computers can be painful.

I don't think that anyone should have to be in a situation where they don't have
access to the setup they want or need. You should be able to plug a USB stick
into a computer, reboot it, and have the environment that you're used to and the
files that you want ready to go.

There are some services that attempt to allow this, but they largely focus on
providing [cloud storage](http://en.wikipedia.org/wiki/Cloud_storage) of
data. 

### Recovery Shouldn't Be Hard For People Who Aren't Gurus
Whenever a system gets into an unusable state, say through file corruption or
important files being deleted, or virus infection, it can be very difficult to
get things back into a known stable state.  Apple's
[Time Machine](http://www.apple.com/support/timemachine/) provides a pretty nice
solution for Mac users, but it can take a very long time, and it's not reliable
for every type of issue.

Recovery partitions, like those used by OSX and Windows 7, can help restore the
system to a basic usable state, but don't help with much of anything else past
that.

It really shouldn't be difficult to get back to **exactly** the setup you want.

### Setting Up a New Machine How You Like It Should Be Incredibly Easy
This is almost the same issue as recovery, but with a more limited scope. When
you buy a new laptop or desktop, for whatever reason, you shouldn't have to do
more than selecting "install my system" from a menu (or similar) to have
everything set up the way you want it. As it is, people generally spend a few
months remembering all the little bits and pieces of software that they used to
use, and re-install them as they remember them.

## You Shouldn't Need to Know Much of Anything About Computers to Utilize a Solution
While I'm planning on implementing this on top of existing tools for a Linux
distribution, it shouldn't matter whether or not you even know what that is. For
the vast majority of users, their computer is something that runs some
particular applications, and has some particular files, and perhaps has some
settings on it that help make it "theirs".

Look at the popularity of "app stores" like
[the Mac App Store](http://www.apple.com/osx/apps/app-store.html),
[Google Play](http://www.android.com/apps/), and
[the Ubuntu apps directory](https://apps.ubuntu.com/cat/). A large part of the
reason they're popular (other than sometimes being the only legitimate way to
get software onto a device) is that they let most users focus on getting
software on their device that fills a particular need. I think that most user's
entire computing stack should be capable of being generated in a similar way to
finding and installing a piece of software from an app store.

The vast majority of users don't know or care to know about partitioning
schemes, bootloaders, tools like chef, internationalization issues, and a
million other things -- and they **shouldn't**. Furthermore, they should still
be able to have everything they want or need available to them wherever they
are, and they should be able to specify what they want without needing to learn
a massive amount of stuff about the way computers work. Ideally, they could be
someone who has never heard of a "package manager" before and still be able to
use a service that lets them setup their machine with Firefox and Digsby with their
accounts loaded on them painlessly.

I think that this is can be accomplished by creating a service that focuses on
providing a clean interface to app selection and allowing user-data to be
uploaded/specified and then creates live and install images that contain those
things. 

This is possibly the hardest part to convey about what I'd like to create. The
solution to the problems I'd like to solve is a technical solution, and I'm very
used to talking about technical solutions in terms of the implementation of
those solutions, and that type of phrasing just doesn't cut it when trying to
show the potential value to someone who hasn't spent the last decade of their
life immersing themselves in neckbeard lore.

In short: I don't think that **anyone** should have to let out a large mental
sigh when they think about setting up a new computer, nor should they be
inconvenienced by not remembering their laptop as long as there's a machine they
can use nearby. All they should need to know is that they can plug the USB stick
they have on their keychain into a machine, do one or two things, and then have
access to what they need or have their new machine set up how they like it.

## Existing Tools and Why They Don't Solve The Problems

Before getting started on this section, I'd like to mention that I don't think
that any of these tools are bad tools. I think they all have their place, and I
think that for the most part they're well-done. That said, I don't think any of
them solve the problems I'm trying to solve.

### [SUSE Studio](http://susestudio.com/)

This is one of the closest things I've seen to being the service I want to
create. It's interface is clean, and it works very well -- if you're in the set
of people who need it. A major reason that it doesn't do what I want to do is
that it requires that the user of it know **a lot** about the underlying
platform.

When you start creating a SUSE Studio "appliance", you see this:

{% img /images/suse-studio/type-of-application.png %}

It's a nice interface, and a sane default selection, but there's a whole lot of
options there that the average user doesn't know anything about. Different
versions of SUSE? Gnome or KDE? A server? 32 or 64 bit?

After selecting images and giving a name to what we want to do, we move on to
software selection:

{% img /images/suse-studio/software-selection.png %}

The same issue presents itself here. What's a repository? An RPM? I do think
that this is the sort of thing that should be *available* for customization by
the user, but I don't think it's the sort of thing that should appear by
default. 

In order to select nethack to be installed, I had to add a repository, then
filter the search results to limit it to the repository I just added because
there were a lot of results for the search.

After adding nethack, we move on to configuration:

{% img /images/suse-studio/general-configuration.png %}

This is really a very nice interface, and is something similar to what I would
expose to a user that wanted to see this stuff. One issue is that when adding a
user and choosing a default shell that hasn't been selected prompts you to add
it, I'd rather take the route of informing the user that it's been added.

The only part of this that I'd expose by default in the service I'm trying to
make would be allowing the username of the default user to be changed.

The "Personalize" section allowing you to upload or choose a logo and background
image is well-done.

The "Startup" section asks about runlevels and EULAs the person booting should
have to agree to. This is not something I would expose by default.

There's a "Server" section (in the setup for the appliance type I chose, the
default one) that would be largely unnecessary for the service I want to
create. 

The "Desktop" section allows you to choose a user to auto-login and specify
startup programs. This is something I wouldn't expose at all by default.

The "Appliance" and "Scripts" sections all require much more knowledge to know
what to do with than I'd be comfortable exposing by default, although they do
provide sane default choices.

In the "Files" section, I had to manually specify the permissions and ownership
information for the files I wanted to be present, and the interface requires
some knowledge of the Linux Filesystem Hierarchy. Other than that, it's
well-done.

The "Build" tab provides a bunch of options that aren't going to be relevant for
the average user, but the feedback provided while building was very nice. When I
selected an "ISO", I didn't realize that I was going to have to click "build
additional" to get my ISO, instead of being delivered the raw image that was
selected as a default.

When booting the iso provided, it didn't auto-login as the user or login by
default to a graphical interface, both of which were options I selected, and
nethack wouldn't run in the live environment due to permissions issues. The
shell interface logged off periodically, and I didn't get to the point where I
installed a real environment to check if nethack was there because I have no
idea what I'm supposed to do to do that, and had already spent about an hour
fiddling around.

In short, minus the ISO not working as expected, SUSE studio is targeted at
people who use and love SUSE, and want to make special-purpose SUSE builds, not
at the problems I'm trying to solve.

### [slax](http://www.slax.org/)

Slax is another project that is similar on a high-level, but also suffers from
requiring a lot of knowledge from the user. I also couldn't find an obvious way
to add data to the build I made. The iso I generated included nethack in the
live environment, but I didn't see an easy way to do a to-disk install.

### Other Tools

Some other tools that were mentioned that might be similar to what I'm trying to
do, including [anaconda](http://fedoraproject.org/wiki/Anaconda),
[woof](http://puppylinux.org/wikka/woof), [pizza](http://pizza.slitaz.org/),
[T2](http://www.t2-project.org/), [UCK](http://uck.sourceforge.net/),
[Remastersys](http://www.remastersys.com/), and
[MyLiveCD](http://sourceforge.net/projects/mylivecd/).

All of the above tools require some combination of needing the user to be
already running a (often specific) Linux distro to run and needing the user to
really understand what's happening at a lower level. Many of them also don't
offer a way to take the applications and user-specified configuration in the
live environment and include them in a to-disk install.

Most of them, if not all of them are trying to solve a different core problem or
problems from the set of problems I'm trying to solve. They tend to be
explicitly allowing a user to create custom live environments, or create very
fine-grained and specially purposed customizations of specific distributions.

### Summary

I'm not aware of any existing tools that address the problems I'm trying to
address. While my project would have a similar implementation to some of the
tools mentioned, it would have a very different focus.

## Conclusion

From a technical standpoint, the generation of a media containing some set of
software that allows an installation to be made that has the same applications
and user configuration as the live environment isn't incredibly hard to
solve. The challenging technical issues largely stem from the resource-intensive
nature of image generation and hosting. If people want to hear about all that
sooner rather than later, I can write about it.

The other, and larger side of the coin here is focusing on creating an interface
that the average computer user can make use of. I plan on making the initial
interface very close to the interfaces that existing app stores used -- the
focus should be on what applications the user needs to be able to run, and with
what configuration. I'd like to be as objective about interface issues as
possible, and would be looking at having someone who's passionate about
[UXD](http://en.wikipedia.org/wiki/User_experience_design)  work on the
interface, and doing [A/B Testing](http://en.wikipedia.org/wiki/A/B_testing)
with different interface ideas and before rolling out interface updates to
existing users. I'm also looking at creating some tools for checking an existing
system for installed software to help create and suggest defaults for
generation.

This is something I've wanted to do, at least on a high level, for almost all of
my time using computers in a non-trivial fashion. I've got the skill to make it
a reality, but I can't afford to give it the attention it deserves without
finances. If I was in a better place financially, I'd fund the development of
this myself -- and if I fail to acquire funding for it in the near future, I'll
pursue it if and when I'm in a place to do so.

That said, it would be absolutely awesome if the community as a whole decided
that this is a service they'd rather see sooner than later -- if you feel that
way, do us all a big favor and
[support the indiegogo campaign](http://www.indiegogo.com/customized-linux), and
tell other people that you think would be interested!


