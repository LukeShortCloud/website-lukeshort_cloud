---
title: 2021-04-18
date: 2021-04-18 12:00:00
categories:
  - kubernetes
tags:
  - kubernetes
  - learning
  - linux
---

[Back to Kubernetes blog.](../#kubernetes)

# Free Ways to Use and Learn Kubernetes

{{< image src="../../images/blog/kubernetes/2021-04-18-free-kubernetes.jpg" alt="Person reading on top of a pile of coins" set="fit" >}}

You don’t even have to spend a dime!

While doing additional research for this blog post, I came across a very cool project. Let me introduce you to [Free Kubernetes](https://github.com/learnk8s/free-kubernetes). This git project contains a variety of ways you can run your own Kubernetes cluster for free. Most of these use public clouds so you don’t even need to worry about requiring any hardware.

For my own learning and growth, I’ve found these to be great tools and hope you do as well!

- Cloud:
    - [Katakoda Playground](https://www.katacoda.com/courses/kubernetes/playground) = This is all you need. You get a single Control Plane Node and a single Worker Node for 1 hour.
        - Once you get good at Kubernetes, you can even use this to create your own training. Companies such as KodeKloud built Kubernetes courses using it.
    - [Civo](https://www.civo.com/) = Civo uses k3s in the back-end. You can get a highly-available cluster in literally a few minutes. No exaggeration! Their Kubernetes service used to offer $70/month for free for its beta program. That’s now ending and they’re instead offering a one-time $250 credit.
        - Using "Small" Nodes, you could get 8 months with three Nodes or 25 months with a single Node!
- Local workstation/server:
    - [Minikube](https://minikube.sigs.k8s.io/docs/start/)/[Minishift](https://www.okd.io/minishift/) = Honestly, I haven’t used these much because they’re so limited out-of-the-box. That being said, this is easy and it works. They provide a golden virtual machine image of a working all-in-one (Controle Plane + Worker) Kubernetes Node.
    - [k3s](https://rancher.com/docs/k3s/latest/en/) = The ultimate home lab tool for your Raspberry Pi cluster. A single binary, one minute install, and one minute upgrades. What’s not to love?
    - [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) = The official tool for installing Kubernetes. This is important to know! Even many third-party Kubernetes installers, such as VMware's Tanzu, are built on-top of this. It's also featured in the Certified Kubernetes Administrator (CKA) exam. Spin up a virtual machine with Vagrant and hack around with the tool.

Bonus:

- [AWS Lamba](https://aws.amazon.com/lambda/?did=ft_card&trk=ft_card) = I’m not 100% sure on what’s used in the back-end. I would guess Kubernetes and honestly it doesn’t matter. This concept of “function-as-a-service” or “serverless” is a huge topic in Kubernetes and I’d argue it’s the next big thing. You can learn the concepts of it with the [AWS Free Tier](https://aws.amazon.com/free/) and later apply it to Kubernetes via the use of [Knative](https://knative.dev/) or [OpenFaaS](https://www.openfaas.com/). You can use 1 million requests or 3.2 million compute seconds. Whichever comes first.

Have any questions about setting up a lab? Reach out to me at @ekultails and I’ll see if I can help! - Luke Short
