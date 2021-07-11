---
title: 2021-01-31
date: 2021-01-31 12:00:00
categories:
  - kubernetes
tags:
  - opensource
  - openstack
  - kubernetes
---

[Back to Kubernetes blog.](../#kubernetes)

# The State of Kubernetes According to One of Its Creators

{{< image src="../../images/blog/kubernetes/2021-01-31-state-of-kubernetes.jpg" alt="Containers and Clouds" set="fit" >}}

Recently, there was a [webinar Q&A session with Joe Beda](https://www.brighttalk.com/webcast/14883/441914/ask-me-anything-with-joe-beda-co-creator-of-kubernetes). He's one of the authors of “Kubernetes Up & Running”. Here are his thoughts on the past, present, and future of Kubernetes.

Note: this information has been paraphrased.

Joe Beda's thoughts:

- The #1 goal for Kubernetes it to become boring and “just work”. It’s almost there!
- How can someone learn Kubernetes?
    - Start small and focus on vanilla Kubernetes. Don’t start off trying to learn a very vendor-specific product like OpenShift. If you learn pure Kubernetes, you can seamlessly migrate between different clouds.
- Was YAML supposed to be the primary way to interact with and use Kubernetes?
    - No, YAML had a long-term goal of being replaced but never was. There are a handful of tools out there that make applications on Kubernetes easier to manage. The first evolution of the interaction with the API was Helm. ytt is now the second evolution.
- What are your thoughts on Platform-as-a-Service (PaaS) offerings such as “low-code” and “no-code”?
    - They are too restrictive and won’t provide developers all the features they need. Eventually, they will need to migrate to Kubernetes where they will have more power, control, and features.
- What is the future of PaaS?
    - Serverless and other frameworks built ontop of Kubernetes will replace the traditional PaaS services we know today.
- What is the future of Infrastructure-as-a-Service (IaaS)?
    - Managing Kubernetes on baremetal is difficult. The best way to manage it is with programmable infrastructure (via IaaS APIs). Platforms such as Amazon EC2 and OpenStack Nova will still be needed for that.
- What are the top problems for developers using Kubernetes today?
    - Networking. The Service and Ingress APIs have created a lot of confusion for developers on how to expose an application to the Internet. A new unified networking API is being worked on in the upstream Kubernetes community to help with this.
- How can developers closely replicate their production (prod) environment as a development  (dev) environment?
    - The drift between prod and dev leads to inconsistencies which leads to bugs. Things that need to be aligned as much as possible: node count, networking, storage, and external (non-Kubernetes) services integration. For at least address the node count, the Kind and Cluster API projects provide a seamless way to spin-up Kubernetes clouds of any size instantly using docker containers. There is no real solution for everything else.
- What is Isito?
    - Features: secure communication between Pods, dynamic routing, observability, and service mesh configurability.
- Why is Istio not part of Kubernetes?
    - Google wants full control over the project. There are also many other Kubernetes plugins in the open source community that solve similar problems usch as Open Service Mesh and Linkerd.
- What book are you reading right now?
    - Range. It explains how generalist can succeed with not well defined problems. It’s about identifying larger patterns/issues and how different things can come together to solve those common issues.
- If people aren’t using a specific Kubernetes API or a functionality of it, maybe we did it wrong. We need to rethink it and get community feedback on how to make all APIs useful.
- What is the future of Kubernetes?
    - GPUs (A.I./M.L.) are gaining a lot of traction right now. However, Kubernetes does not solve every problem. What it did great was embrace the idea of declarative infrastructure state. In 10-20 years, there may be a similar declarative tool and it may not be Kubernetes.

My thoughts and biggest takeaways:

- It's fascinating that YAML was not meant to be the long-term solution to using Kubernetes. Helm was an amazing leap forward for deploying applications and has been a joy for me to work with over the years. I'm now starting to get my hands dirty with ytt on a few projects I'm working on so we'll see how that compares.
- I love the attitude of if the end-users aren't using the APIs then it was probably designed or implemented wrong. It's important to fail fast, gather feedback, and then iterate again.
- For development environments, I love Kind. It's so easy to get started with. It's a much better experience than trying to lab OpenShift (outside of the limited Minishift environment).
    - Check out my notes on how to [use Kind](https://github.com/ekultails/rootpages/blob/master/src/virtualization/kubernetes_administration.rst#kind) and [configure the related Cluster API](https://github.com/ekultails/rootpages/blob/master/src/virtualization/kubernetes_development.rst#id13).
- People always wrongly assume projects like OpenStack are dead. They aren't. They're just boring and work. That's where Kubernetes is heading. Arguably, I actually think Kubernetes reached the boring state about two years ago. IaaS is important and it's not a problem Kubernetes tries to solve. It needs IaaS to prosper.
- The future of technology, in my eyes, has always been serverless and machine learning. Joe seems to echo these thoughts which makes me feel even more confident in my choice of career. At work, I'm focusing on making those my niche so that I may help customers adopt these concepts and get the most out of Kubernetes.
