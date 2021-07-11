---
title: 2020-12-14
date: 2020-12-14 12:00:00
categories:
  - operatingsystems
tags:
  - centos
  - cloudlinuxos
  - enterpriselinux
  - oraclelinux
  - redhat
  - rhel
  - rockylinux
---

[Back to Linux blog.](../#linux)

# 3 FREE Drop-in Replacements for RHEL and CentOS 8

Wow. Out of nowhere, CentOS 8 just annouced it'll be going [end-of-life next year](https://blog.centos.org/2020/12/future-is-centos-stream/). It was supposed to recieve updates until the year 2029.

Here are three alternatives to checkout. Oracle Linux is the only one you can download and use today. The others are a work-in-progress. All of these can be used as a drop-in replacement. A simple change of the repository URLs should be all that's required to switch to one of these.

1. [CloudLinux OS](https://blog.cloudlinux.com/announcing-open-sourced-community-driven-rhel-fork-by-cloudlinux) = CloudLinux brings its years of experience and infrastructure to help create a CentOS 8 replacement.
2. [Rocky Linux](https://rockylinux.org/) = Created by one of the original founders of CentOS.
3. [Oracle Linux](https://linux.oracle.com/switch/centos/) = A free alternative to RHEL that provides additional features including the "Unbreakable Linux" kernel.

CentOS as a project is not ending. CentOS 7 will continue to get full support until 2024. CentOS Stream is being pitched as the official replacement to CentOS 8. This more-so aligns with how Red Hat handles their open source projects: there is a leading-edge open-source technology that gets polished and turned into a product. This means that CentOS Stream will be an unstable project since it has the latest (barely tested) changes. I speak from experience that there's a HUGE difference between a stable CentOS 8.Y point release and a rolling release. I will elaborate on this in a follow-up blog.

I can no longer recommend it as a production operating system to use. Can/should you use CentOS Stream for CI/CD for a product that's being deployed on RHEL? Sure, tack it on as another distro. Make sure you also use Oracle Linux or another alternative to test the stable point releases, though, if you cannot use RHEL itself due to licensing/financial constraints. RHEL 8 also provides free-to-use container images via the [Universal Base Images (UBIs)](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image). CentOS Stream will still, at least, more closely align with RHEL than Fedora does.

Good luck with the migrations! R.I.P. CentOS. 
