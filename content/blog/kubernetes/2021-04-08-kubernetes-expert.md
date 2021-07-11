---
title: 2021-04-08
date: 2021-04-08 12:00:00
categories:
  - kubernetes
tags:
  - kubernetes
  - learning
  - linux
---

[Back to Kubernetes blog.](../#kubernetes)

# How You Can Become a Kubernetes Expert

{{< image src="../../images/blog/kubernetes/2021-04-08-kubernetes-expert.jpg" alt="Ocean shipyard with containers" set="fit" >}}

Here’s how.

- YAML = What do Kubernetes, Ansible, and GItHub Actions have in common? YAML! It’s important. My colleagues and I constantly joke about how we are YAML Engineers(™). Don’t be intimated! It’s so popular because it was made to be easy and human-readable.
- Containers = Do you know and understand what containers are and how they work?
    - No? Start here before you go any further. Kubernetes is an automation and orchestration platform of sorts. You need to understand the underlying technology first before you start adding on extra layers of features and complexity. Checkout the [KodeKloud Docker for Beginners course](https://www.youtube.com/watch?v=zJ6WbK9zFpI).
    - Yes? Great! Moving on.
- Vanilla Kubernetes = A quick note! This is a mistake that I’ve made and many others make. Do NOT start by studying a vendor-specific implementation such as OpenShift. They are extremely biased and have features that are not portable across other Kubernetes clusters. OpenShift, in particular, is overly complex. It tries to give you everything including the kitchen sink. If you learn Kubernetes then, by extension, you’ll learn OpenShift. The same cannot be said for the other way around.
- Kubernetes basics = Learn the basics of Kubernetes. VMware has free training on [KubeAcademy](https://kube.academy/) that does a great job of going through those fundamentals.
- APIs = Don’t sweat how to install Kubernetes or how it works. Focus on using the APIs which is as simple as writing some YAML manifests. I’ll be writing future tutorials on my blog to help out here.
    - [KataKode Kubernetes Playground](https://www.katacoda.com/courses/kubernetes/playground)
    - [Most commonly used Kubernetes APIs](https://ekultails.github.io/rootpages/virtualization/kubernetes_development.html#popular-apis)
    - [Kubernetes 1.20 API Reference Docs](https://v1-20.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/)
- Community group = Okay, now you know enough to be dangerous. Nice! Find fellow friends, coworkers, even strangers! Just anyone who wants to learn Kubernetes and gets excited by it! Having others around to help motivate you is more powerful than anything else.
- Find use cases = Figure out how you could use Kubernetes at home or for work. Try to move existing apps you use into containers (if not already) and then into Kubernetes. Here are some example use-cases for real applications I run on my home Kubernetes cluster:
    - Application development = I’m a cloud native developer at heart so when I’m testing my apps, I test them as containers on my Chromebook and then push them to Kubernetes.
    - CI/CD
    - DNS
    - Gitea
    - Blog (staging area)
    - CIFS server
    - NFS server
    - Game servers (ex., Minecraft, Halo Custom Edition) = These aren't very cloud-native but they're fun to get working!
- Certifications = Studying for the Kubernetes certifications is a great goal to set for yourself. You’ll learn a lot and have the credentials to help you get that shiny new Kubernetes job! There are currently 3 different certs: the Administrator, Application Developer, and (recently released) Security. Pick your own adventure!
- Going beyond = The final step is to go to, as Buzz Lightyear from Toy Story would say, “to infinity and beyond!” These are a collection a great resources to help you learn about extra features you can add on Kubernetes. Use this as an opportunity to find your niche(s).
    - My (Luke Short) Kubernetes notes
        - [Kubernetes Administration](https://ekultails.github.io/rootpages/virtualization/kubernetes_administration.html)
        - [Kubernetes Development](https://ekultails.github.io/rootpages/virtualization/kubernetes_development.html)
    - [Awesome Kubernetes Resources (Guide to Guides)](https://github.com/tomhuang12/awesome-k8s-resources)
    - [TGI Kubernetes YouTube Livestreams](https://www.youtube.com/playlist?list=PL7bmigfV0EqQzxcNpmcdTJ9eFRPBe-iZa) 
    - [Kubernetes Podcast](https://kubernetespodcast.com/)

My final thoughts are this: Kubernetes is a lot easier than you think. You can do anything if you put your mind to it! I wish you the best with your Kubernetes adventure! - Luke Short
