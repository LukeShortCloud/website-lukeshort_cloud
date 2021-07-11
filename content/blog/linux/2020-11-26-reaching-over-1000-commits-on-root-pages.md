---
title: 2020-11-26
date: 2020-11-26 12:00:00
categories:
  - rootpages
tags:
  - rootpages
  - notes
  - opensource
  - openstack
  - kubernetes
  - programming
---

[Back to Linux blog.](../#linux)

# Reaching Over 1,000 Commits On Root Pages!

Over four years ago, I started one of my biggest open source projects ever: my notes.
This is not your typical git project.
It is not some fancy cloud-native application with an API.
This is [Root Pages](https://github.com/ekultails/rootpages).


At any given time, I like to say that 70% of my technical knowledge is retained within those notes and the GitHub issues page.
It was a spiritual successor to my proprietary notes (dubbed "The Jedi Knight Archive").
The goal is simple: I want to share everything I learn with others.


This week I passed over the one thousand commit mark!
It is now officially the largest project I have ever created.
Let's take a walk down the git history lane to see what I have taken the most notes on.

```
$ git log | grep -o -P '\[.+\]\[+.+\]+' | sort | uniq -c | sort -rn | head -n 20
     86 [openstack][tripleo]
     39 [virtualization][kubernetes]
     30 [virtualization][openstack]
     24 [commands][virtualization]
     23 [automation][ansible]
     23 [administration][chromebook]
     19 [programming][go]
     15 [virtualization][kubernetes_administration]
     14 [programming][devops]
     13 [openstack][developer]
     13 [computer_hardware][monitors]
     13 [commands][package_managers]
     12 [virtualization][containers]
     10 [administration][operating_systems]
      8 [commands][openstack]
      7 [virtualization][virtual_machines]
      7 [virtualization][kubernetes_development]
      7 [linux_commands][virtualization]
      7 [administration][graphics]
      6 [virtualization][wine]
```

Looking at the top 20 commits here, we get an idea of what the majority of my notes are. Due to constant restructuring of the pages and not always using these ``[tags]`` in my commit messages, this is not completely accurate. It is still fair to say that I spend most of my time working on and learning about OpenStack, Kubernetes, and programming.

1. OpenStack = 86 [openstack][tripleo] + 30 [virtualization][openstack] + 13 [openstack][developer] + 8 [commands][openstack] = 137
2. Kubernetes = 39 [virtualization][kubernetes] + 15 [virtualization][kubernetes_administration] + 7 [virtualization][kubernetes_development] = 61
3. Programming = 19 [programming][go] + 14 [programming][devops] = 33

Searching the whole git history reveals a more accurate number of my notes with programming taking the lead over Kubernetes. That makes sense as I have been programming for over 6 years. Kubernetes has only been a large part of my life for 3. OpenStack has been one of my primary job responsibilities for the past 5 years so there are a lot of ins and outs I have discovered.

```
$ git log | grep '\[' | grep -P "kolla|openstack|tripleo" | wc -l
233
$ git log | grep '\[' | grep -P "c\+\+|devops|go|packaging|programming|python" | wc -l
103
$ git log | grep '\[' | grep -P "containers|kubernetes" | wc -l
87
```

Are you curious to learn more about the cloud and technology industry? Feel free to check out my notes, fork them, open GitHub issues, or whatever your heart desires! You can check them out [here](https://ekultails.github.io/rootpages/). My notes are published online at the end of every quarter. 

---

By the way, the day I'm publishing this is Thanksgiving! I'm thankful for the continued support of my friends, family, and colleagues. It's their motivation and drive that helps to spark my own fire.


I'm now off to eat some Tofurkey (trust me, if you cook *anything* right and use the correct seasonings it will be delicious)! Cheers!
