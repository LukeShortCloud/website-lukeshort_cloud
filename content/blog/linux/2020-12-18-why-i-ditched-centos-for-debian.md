---
title: 2020-12-18
date: 2020-12-18 12:00:00
categories:
  - operatingsystems
tags:
  - canonical
  - centos
  - cloudlinuxos
  - debian
  - enterpriselinux
  - oraclelinux
  - redhat
  - rhel
  - rockylinux
---

[Back to Linux blog.](../#linux)

# Why I Ditched CentOS for Debian

{{< image src="../../images/blog/linux/2020-12-18-why-i-ditched-centos-for-debian.jpg" alt="GNU Linux penguin with Red Hat Fedora on" set="fit" >}}

Earlier this year, [even before CentOS 8 shortened its life to transition to Stream](../linux/#2020-12-14), I moved all of my servers off of CentOS and switched to Debian. Let me explain why.

*Disclaimer: Even though I work(ed) for Red Hat, I was never involved with the RHEL team nor do I have any information on the direction of the product or the CentOS project.*

- **Arm support.** This came too little, too late. It took almost an entire year after CentOS 8 came out for an Arm image to be provided. Debian currently has support for [9 different CPU arhitectures, incluing Arm](https://wiki.debian.org/SupportedArchitectures). For a long time, I painstakingly was the only maintainer of a (hacky) build of [CentOS 8 and Fedora for the Rock64 and Pine64 devices](https://github.com/ekultails/rock64_fedora). It was not worth the time investment as I rather spend time on other projects and studying new technologies. I also ditched the Rock64 devices for the Raspberry Pi. You will get no better software and hardware support than with the original single-board computer that started all of the "clone wars."
    - By the way, the [correct way to spell Arm is "Arm" not "ARM"](https://www.arm.com/company/policies/trademarks/guidelines-trademarks).
- **Kubernetes support.** The release of Enterprise Linux (EL) 8 completely removed the legacy iptables package in favor of pushing for nftables usage instead. This has caused many headaches for me as most software out there, including Kubernetes, relied on it. I did a lot of research in the past and found that the Linux kernel it ships still has the necessary patches for legacy iptables to work. There's simply no `iptables-legacy` package/binary to install. [Kubernetes support is still limited to RHEL/CentOS 7 right now](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/). Heck, even the open source version of OpenShift (OKD) did not work on EL 8 until a few months ago! Other Kubernetes distributions, such as k3s, have limited support on EL 8 which (1) requires that there's no firewall enabled and (2) kube-proxy will not work properly.
- **Btrfs support.** RHEL 7, and by extension CentOS 7, has [deprecated the Btrfs file system](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-btrfs) and removed it from EL 8. This is my favorite go-to free and open-source software (FOSS) Linux file system. I heavily use the snapshot functionality to take quick and easy backups. Red Hat simply has no Btrfs engineers so I understand why they made the move. Hopefully, now that [Fedora supports Btrfs by default](https://fedoraproject.org/wiki/Changes/BtrfsByDefault), things may change in the future.
- **Upgrade path.** CentOS has no upgrade path. No official one, anyways. Users have reported mixed results of going from CentOS 7 to 8 by simply updating the repository URLs. Only RHEL has an upgrade path and I do not use that distribution except for work purposes (i.e., not my personal projects).
- **Licensing.** Take this with a grain of salt as I have no supporting evidence for this. I have heard from a few different colleagues from different companies that if you modify CentOS or run Ubuntu at a large-scale then either Red Hat (CentOS) or Canonical (Ubuntu) will go after you seeking licensing compensation. Although Debian is backed by Canonical, it's a truly a FOSS operating system with no strings attached. I also do not use RHEL itself because I do not want to deal with subscriptions.
- **Compatibility.** Here's a fun fact that many people don't know: CentOS != RHEL. What I mean is that it's common for packages to be different. The biggest headace I ran into was in the OpenStack community. We had a hell of a time trying to use CentOS 8.0. It was based on an old beta of RHEL 8.0 while RHEL 8.0 continued to get major package updates. More specifically, the [Container Tools](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/building_running_and_managing_containers/index) Podman package (Red Hat's replacement for the Docker Engine) was stuck at an older version in CentOS and was updated at the last minute in RHEL before the release. CentOS did NOT resync with RHEL again until the 8.1 release. We had to write a lot of workaround patches. `if CentOS: apply_stupid_and_complex_fix()`. Debian is Debian. No ifs, ands, or buts. I'll leave this point with a question for you to ponder: is software that's older more stable or is newer more stable?
- **Toy Story.** This wasn't really a deciding factor, just some fun trivia. Each Debian release name is based on a Toy Story character. In fact, even the [logo is literally the chin of Buzz Lightyear](https://www.reddit.com/r/linuxmasterrace/comments/4bp7rk/til_that_this_was_the_inspiration_for_the_debian/)! Toy Story's one of my favorite Disney movies and Debian Wheezy was the first release of Debian I ever tried so it has a special place in my heart. Back when my girlfriend (now wife) and I were celebrating an anniversary at Disney World, we found a Wheezy stuffed animal. I had to have him and my girlfriend graciously bought him for me. He's been with me ever since. :-)
    - By the way, is anyone else excited for the upcoming [Buzz] [Lightyear movie from Pixar](https://www.youtube.com/watch?v=FatABTRIDvw)?!

What do you think? Am I justified in leaving CentOS? I have no regrets so far!
