---
title: 2021-05-16
date: 2021-05-16 12:00:00
categories:
  - chromebooks
tags:
  - android
  - chromeos
  - crouton
  - crostini
  - laptop
  - linux
---

[Back to Chrome OS blog.](../#chrome-os)

# CloudReady is Becoming Chrome OS

{{< image src="../../images/blog/chrome-os/2021-05-16-cloudready-becoming-chrome-os.jpg" alt="Chromebook loading page" set="fit" >}}

The two big Chromium OS players are combining into one streamlined product.

## What's CloudReady?

CloudReady is a fork of Chromium OS (the open source base for Chrome OS) made by a company called Neverware. The value proposition behind it is that you can revive old devices and make them faster by installing Chromium OS. Basically turning devices into DIY Chromebooks/Chromeboxes. At the very end of last year, they have been bought out by Google. Lots of changes have been in the works with more to come.

## What Has Been Done So Far?

CloudReady has already made a lot of changes recently:

- CloudReady code has been fully integrated into Chromium OS 90.
- Partitions went from 27 to 12 to align with Chrome OS.
- Flatpak is removed.
- Python is removed.
- VirtualBox is removed.

## What About Alternative Solutions To Those Missing Software?

Never fear, there are alternatives to everything that have been removed:

- Flatpak = Use the official Linux/Crositini environment to install Flatpaks.
- Python = Install with Chromebrew or use the official Linux/Crostini environment to install all of your required packages.
- VirtualBox = Chrome OS supports nested virtualization. In Linux/Crostini, install “virt-manager” to easily setup KVM virtual machines easily. I have a future blog post incoming for how to use that. Stay tuned! :-)

## What's Missing In CloudReady Compared To Chrome OS?

Only a few pieces are still missing from CloudReady. These include:

- Android = There is no Android support in CloudReady. Although Android itself is an open source operating system, the goodies we all rely on that are the essential Google Apps (including the Play Store) are proprietary and present a legal gray area. The company behind CloudReady has decided to stay safe by avoiding it.
- Powerwash = Powerwash (the Chromium OS reset feature) is biased towards Chrome OS. CloudReady currently requires a full re-installation to “powerwash” the operating system.
- Verified boot = CloudReady cannot verify that everything from hardware to software is correctly managed as it is not a Chromebook. It’s designed to be generic enough that it works on most desktop and laptop computers.
- Canary update channel = Only the Stable, Beta, and Dev update channels exist. The cutting-edge Canary channel is not available on CloudReady.

## What to expect in the future?

Prediction time! In short: expect most of those missing features to come to CloudReady. Google is in a great position to take over the market with Chromebooks and, now, with legacy and home-built hardware if they provide a complete Chrome OS experience with CloudReady. Google I/O is a few days out. If we’re lucky, we’ll hear more about their ultimate plans there!

\- Luke Short

Sources:

- [About Chromebooks - Google is quickly working to integrate Neverware CloudReady into Chrome OS 90](https://www.aboutchromebooks.com/news/google-is-quickly-working-to-integrate-neverware-cloudready-into-chrome-os-90/)
- [Neverware - CloudReady v89.4 now available!](https://www.neverware.com/blogcontent/2021/05/11/update-cloudready-v894-released)
- [ChromeReady - CloudReady vs. Chrome OS: Key Differences](https://chromeready.com/2373/cloudready-vs-chrome-os-key-differences/)
- [Neverware - Switching CloudReady Release Channels](https://cloudreadykb.neverware.com/s/article/Switching-CloudReady-Release-Channels)
