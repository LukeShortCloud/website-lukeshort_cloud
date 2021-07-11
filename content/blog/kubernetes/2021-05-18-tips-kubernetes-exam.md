---
title: 2021-05-18
date: 2021-05-18 12:00:00
categories:
  - kubernetes
tags:
  - cka
  - ckad
  - cks
  - kubernetes
  - learning
  - linux
---

[Back to Kubernetes blog.](../#kubernetes)

# Tips to Help You Pass Any Kubernetes Exam

{{< image src="../../images/blog/kubernetes/2021-05-18-tips-kubernetes-exam.jpg" alt="Person nervously biting pencil while studying" set="fit" >}}

Trying to get your CKA, CKAD, and/or CKS certification for fun, profit, and/or glory? Here are the top tips for success based on my own experiences!

## Exams Order/Priority

All of the exams are very similar and build off one another. Assuming your goal is to obtain all of the certifications, tackle them in the order below. This provides a clear path of adding on additional APIs and tools one exam at a time. That means that, for example, taking the CKA before the CKAD would probably be a harder experience.

1. Certified Kubernetes Application Developer (CKAD)
2. Certified Kubernetes Administrator (CKA)
3. Certified Kubernetes Security Specialist (CKS)

## Schedule a Date

"If you talk about it, it's a dream. If you envision it, it's possible. But if you schedule it, it's real." - Anthony Robbins. This is hands-down one of my favorite quotes I discovered from a mentoree/mentor of mine.

I found that I was ready for the CKA exam after a few months of studying. My biggest downfall was never scheduling the exam in the first place. I spent extra time over-preparing and trying to always dig deeper. After all, the end-goal wasn't to get a seemingly worthless piece of a paper. It's to build up your skills to help you with your job! Just make sure you have a clear goal to work towards or otherwise you won't have anything to show for it!

Get the certification first and then your time will be freed up to go that extra mile of learning more of the related and advanced Kubernetes topics. If nothing else, this will unlock new job opportunities sooner. Recruiters will be knocking on your door!

## Study Time

If you want to become an expert in anything, you need to devote time every day. No exceptions. I aim for 1 hour a day. Even if I can't commit a full hour, I try to spend a minimum of 30 minutes. On "cheat days" I'll just watch tutorial videos online and not do hands-on. However, the hands-on experience is the most valuable.

## Study Resources

Great, you want to dedicate time every day to learn! Now what?

For learning the primary exam materials, there are no better courses than the ones offered by KodeKloud. Use the free Katakoda Kubernetes Playground to test things you have learned. A couple of weeks before your exams, take a Killer Shell (killer.sh) practice exam. It's designed to be harder than the actual exam to prepare you better. From that, identify areas of improvement and continue to review those sections in KodeKloud and run through the related end-of-chapter practice tests.

- [Katakoda Kubernetes Playground](https://www.katacoda.com/courses/kubernetes/playground)
- [KodeKloud on Udemy](https://www.udemy.com/courses/search/?q=kodekloud)
- [Killer Shell Kubernetes Exam Simulator](https://killer.sh/)

## Bookmarks

Yes, you're allowed to use bookmarks during the exam! More specifically, you're allowed two tabs: one for the exam and the second for searching official Kubernetes resources. From the official Kubernetes websites, I can assure you that the [Kubernetes Documentation](https://kubernetes.io/docs/home/) is all you need. There are lots of great real-world example manifests and hints to be found. Identify your weak areas and bookmark related documentation pages. Consider keeping all of your bookmarks in a single bookmark folder that is clearly visible and easily accessible.

The [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) and [kubectl Command Reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) are great examples of hidden gem bookmarks.

P.S. - You are NOT allowed to have one of your tabs open in a separate window. This means that if you have a large 4K or ultrawide monitor, you won't be able to take full advantage of the extra screen real estate.

## Manifests

Most Kubernetes professionals will tell you to never use `kubectl run`. I'm here to tell you that you should use it *and* use it all the time. The catch is, however, that you should never create objects with it. Instead, use it to create YAML manifests. For the exam, you'll have a copy of it that you can examine later. For work/play, you'll have a manifest you can git commit for the invaluable version control/history.

Say hello to your new friend: `--dry-run=client -o yaml`. This'll output an example YAML manifest. It lays down the foundation so you can then tweak it based on the object you need.

- `kubectl run nginx --image nginx --dry-run=client -o yaml >> pod-nginx.yaml`

**Pods**

Creating Pods on the CLI uses a special command: `kubectl run`. This is intentionally meant to be similar to `docker run`. You don't even need to memorize the options. Most times, you can get away with grabbing the relevant help information:

- `kubectl run --help | grep run`

**Everything Else**

Most of the common APIs can be created via the CLI. View the ones you can create (look for "Available Commands"):

- `kubectl create --help`

Again, do a simple grep to help find pre-made examples of arguments that can be used:

- `kubectl create <API> --help | grep create`

## kubectl

Ah, look what we have here. `kubectl`. Get used to it, my friends. You'll be using this a lot. Here's a brain dump of useful, and not very well-known, commands for the exam and also in the real-world:

- **Shortcuts**

    - `alias k=kubectl` # Save time by not having to type 6 extra letters! This, however, negates bash completion. Pick your kryptonite.
    - `export d="--dry-run=client -o yaml"` # Set a shell variable to save time when creating YAML manifests. Add `$d` to the end of the `kubectl [run|create]` command.

- **API**

    - `kubectl api-resources` # View all of the APIs available.
    - `kubectl api-resources --namespaced` # View all of the APIs that support being namespaced.
    - `kubectl api-versions` # View all of the API versions available.
    - `kubectl explain <API>` --recursive # View all of the available options for a specific API.
    - `kubectl explain <API>.spec` # View the "spec[ification]" field of a specific API.

- **Create, Read, Update, and Delete (CRUD)**

    - `kubectl create <API> | grep create` # View examples of how to create objects.
    - `kubectl get <API> --show-labels` # View the labels for each object.
    - `kubectl get <API> -w` # Watch for updates to a particular resource
    - `kubectl top pod --containers` # View the resource consumption of all containers.
    - `EDITOR=nano kubectl edit --record <OBJECT>` # Update the manifest of a running object using the specified $EDITOR variable. Record the previous object manifest as a single-line annotation.
    - `kubectl delete <API> <OBJECT> --wait=0` # Do not wait for the object deletion to finish. Return back to the shell prompt immediately.

- **Cluster**

    - `kubectl get events -A --sort-by=.metadata.creationTimestamp` # Get all (most, actually, as `-A` only applies to a handful of APIs) events from a cluster ordered by the newest first.
    - `kubectl describe node <NODE> | grep -i cidr` # Find the Pod network CIDR allocated to a specific Node.
    - `kubectl cluster-info dump | grep -- --service` # Find the cluster-wide Service CIDR

- **Role-Based Access Control (RBAC)**

    - `kubectl create sa ...; kubectl create role ...; kubectl create rolebinding ...` # Create a ServiceAccount, a Role which defines which permissions are granted, and active the ServiceAccount by assigning the Role via a RoleBinding (or ClusterRoleBinding).
    - `kubectl auth can-i --list` # View all of the permissions the current user has.
    - `kubectl auth can-i <ACTION> <API> --as system:serviceaccount:<NAMESPACE>:<SERVICEACCOUNT_NAME>` # Verify that a ServiceAccount can perform the specified action.

- **kubeadm**

    - `kubeadm certs check-expiration` # View the TLS certificates expirations for Kubernetes services.
    - `kubeadm certs renew <KUBERNETES_SERVICE>` # Renew a TLS certificate.
    - `kubeadm token create --print-join-command` # Print the command to join a Worker Node. Copy that command and run it on the new Worker Node to add it to the Kubernetes cluster.

## Retake

Worst case scenario, you fail your exam. That's okay becase you get a free retake! Better yet, now you now know what to expect. Think back to the questions you didn't understand and/or took too much time on. Build up your skills from there. Get better at solving those kinds of scenarios and solving them quickly.

## What Next?

You have your certification! Congratulations! Now what?

Find your niche. Heck, you can even use your shiny new certification as an inspiration for your starting point. Here are a few high-level examples (there are many ways to tackle each):

-  Administration = Familiarize yourself a few different deployment tools. Customize the Kubernetes services to expose (or even disable) different features.
-  Application Developer = Find frameworks to help build and deploy cloud-native applications automatically.
-  Security Specialist = Brush up on more advanced RBAC topics and how to lock-down clusters in such a way that any government agency would be proud.

Get a promotion, a new job, or even create a start-up! Sky's the limit for what you want to do with your new skills!

P.S. - VMware, where I currently work, is always hiring for our Kubernetes teams. If you're looking for a job, let me know! We're especially interested in hiring women and people of different races and ethnicities. We've got amazingly ambitious diversity goals! Don't believe me? Read more about our goals [here](https://www.vmware.com/company/diversity.html).

## Closing Thoughts

Even if only one piece of advice helps you on your journey then I'm happy to have written this article. If you truly want a Kubernetes certification, you'll get it.

Reach out to me on Twitter @LukeShortCloud (previously @ekultails) or on LinkedIn if you need any help with your Kubernetes journey. I'd be glad to provide guidance!

- Luke Short
