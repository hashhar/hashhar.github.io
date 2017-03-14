---
layout: post
title: "Arch Linux Step 0: An Introduction"
date: 2017-03-14 03:59:55 +0530
categories: ArchLinux,setup,distro,linux
---
Wow, it's been almost over a month since my last blog post. I've taken quite a few steps to improve this situation and
hopefully my newer posts will be more routine and regular. With that said, let's cut to the chase.

## What is Arch Linux?

Glad you asked. [Arch Linux][] is a lightweight and flexible rolling release Linux distribution that tries to Keep It
Simple meaning that you get a lean and mean system free of any opinionated configuration that is always up to date and
you are free, and even encouraged, to configure it however you want. This means that you get the opportunity to learn
every little thing about your OS and provides you a perspective on Linux that you cannot gain by using distributions
like Ubuntu or its' other flavours.

## How is it different from, say Debian or Fedora?

There are a few key [Arch Linux principles][] which make them quite different from most of the other Linux
distributions.

1. **Simplicity:** It ships software as released by the original developers (upstream) with minimal
   distribution-specific (downstream) changes including the configuration files. It also doesn't change anything in your
   system unless you explicitly tell it to meaning that you have full control.

2. **Rolling Release:** Arch focuses on being up to date to avoid dealing with problems that backward compatibility
   brings. It is a rolling release distribution meaning there's no concept of releases like Ubuntu. Instead once you
   install the OS and periodically update all packages using the package manager you will always be up to date.

3. **Pragmatism:** The large number of packages and build scripts in the various Arch Linux repositories offer free and
   open source software for those who prefer it, as well as proprietary software packages for those who embrace
   functionality over ideology.

## How does it look?

{% include image.html
           img="/images/arch-linux-live.jpg"
           title="Arch Linux live environment"
           caption="This. It just looks like this." %}

But it can go from that to this if you want.

{% include image.html
           img="/images/rXaMQcg.png"
           title="Arch Linux ricing"
           caption="It can also look like this!" %}

{% include image.html
           img="/images/CourageousWiltedKrill-size_restricted.gif"
           title="Arch Linux ricing"
           caption="Even this!" %}

You just get 800MB worth of stuff. It doesn't even include a graphical environment or web browser out of the box. You
have to configure the network manually if you don't have DHCP. Almost everything you can think about doing can only be
done via the command line.

Oh, and did I mention that there's no "Install" button. You have to bootstrap the system from the live environment and
then `chroot` into it and continue the installation from there. It's as involved as it sounds and a little overwhelming
at first but the amount of knowledge you'll gain just from the install process alone will be something you couldn't have
ever gained while using Ubuntu.

Apart from that the [Arch Wiki][] is the best resource you can find should you run into any problems and the [Arch
Forums][] are also a great place to ask for help. There's a certain [Code of Conduct][] that the Arch community follows
and you should be respectful of that before diving into the forum and asking questions. The golden rules are:

1. **Use the wiki Luke!!!:** Seriously. Everything you may need to find or do has been tackled in the wiki. If it's not
   in the wiki, head to the internet before heading to the forums.

2. **Use the internet:** Before jumping into the forums and asking a question make sure that you have at least tried to
   solve it so that you can say something when people ask you what you have already tried?

3. **Be as descriptive as possible and go in with a learning attiude:** Try to describe your problem in as much detail
   as you can and try to learn from what others are telling you.

## What will I learn?

A lot of things. How does Linux keep track of timezones, what is the [RTC][], how is a network configured behind the
scenes, how does the Linux sound system work and in general digital audio, systemd services, filesystem partitioning and
mounting, etc.

## Teach me master!

Now that you have decided to at least try it out, head on over to [step 1][]. Good luck and happy ricing.

There are a lot of similar distros out there. You can also try them if you want. [Gentoo][] is similar to Arch in that
it also a DIY distro but instead of using binary packages it installs software from source. Then there's the ultimate
beast to tame, [Linux From Scratch][] which teaches you to build a Linux distribution *entirely* from scratch -
everything has to be compiled from source.

<!-- Links -->
[Arch Linux]: https://www.archlinux.org
[Arch Linux principles]: https://wiki.archlinux.org/index.php/Arch_Linux
[Arch Wiki]: https://wiki.archlinux.org
[Arch Forums]: https://bbs.archlinux.org
[Code of Conduct]: https://wiki.archlinux.org/index.php/Code_of_conduct
[RTC]: https://en.wikipedia.org/wiki/Real-time_clock
[step 1]: {{ site.url }}/arch-linux-step-1-installation-guide
[Gentoo]: https://www.gentoo.org/
[Linux From Scratch]: http://www.linuxfromscratch.org
