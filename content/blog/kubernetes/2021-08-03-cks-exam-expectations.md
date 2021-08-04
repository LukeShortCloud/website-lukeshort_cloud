---
title: 2021-08-03
date: 2021-08-03 12:00:00
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

# CKS Exam Expectations

{{< image src="../../images/blog/kubernetes/2021-08-03-cks-exam-expectations.jpg" alt="" set="fit" >}}

These are my expectations for the kind of questions to expect on the Certified Kubernetes Security (CKS) exam.

**DISCLAIMER:** These are predictions largely based on the official [CKS Curriculum](https://github.com/cncf/curriculum/blob/cd91baf4c4ec8c830834d46523fcd78c7a3734c6/CKS_Curriculum_%20v1.21.pdf) and courses found on Udemy. I've NOT taken any CKS exams (practice, real, or otherwise). I'm writing my thoughts down now before I start taking practice exams and enter NDA territory. These questions will NOT reflect what is exactly on the exam and are only educated guesses for what we should be prepared for.

## Practice Exam

Without further ado, here is a practice exam I've created that you can use as study material. Do not consider this a comprehensive/complete list of questions. Use it as a starting point.

- Build a container using a Dockerfile with a multi-stage build.
    - The last stage should use the lightweight "alpine" image.
- Create a PodSecurityPolicy to force Pods to be read-only.
- Deploy a WordPress website with a HTTP front-end (nginx) and a database back-end (mysql).
    - Pod configuartion:
        - Use the gVisor/runsc container runtime class.
        - Apply an existing AppArmor profile for the NGINX container.
        - Run as a non-root user.
        - Add the "CAP_SYS_ADMIN" capability.
            - (Production tip: avoid using this capability as it grants near-root levels of access).
        - Disable the ServiceAccount mount.
    - Use NetworkPolicies to only allow traffic to/from the relevant ports (80 and 3306) used by the Pods.
    - Create a self-signed certificate and store it as a Secret object.
    - Create an Ingress object with the TLS certificate.
        - (Lab tip: use "cert-manager" to automate certificate creation).
- Encrypt all existing Secret objects and ensure new Secrets will also be encrypted.
- Use `crictl` to manually access a container running on a worker node.
- Upgrade Kubernetes from one minor version to the next: 1.21.0 to 1.22.0.
- Create a new user account using the ClusterRole, ClusterRoleBinding, and CertificateSigningRequest APIs.
- Create a new ServiceAccount.
- Create a new ConstraintTemplate object with a provided OPA policy.
- Scan the "nginx:1.18.0" image with Trivy.
- Run CIS benchmarks with 'kube-bench'.
- Run a system scan with Falco.
- Find all non-Kubernetes systemd services and stop them.
- Enable audit logging in the kupe-apiserver.
    - Identify which users are interacting with the API.
- Enable ImagePolicyWebhook in the kube-apiserver.
    - Allow the container image "nginx:1.18.0" to be used on the Kubernetes cluster.
- Verify the checksum of the binaries installed with those mentioned in the official [Kubernetes change log](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md).

If you can do everything from above, you're most likely in a good spot to get a passing score.

## Parting Words

The CKS builds on-top of concepts from what the Certified Kubernetes Administrator (CKA) exam is about. That also makes it the most challenging exam the Cloud Native Computing Foundation (CNCF) has made to date. Only take the CKS if you've already passed the CKA.

Be sure to also check out my related guide on [Tips to Help You Pass Any Kubernetes Exam](https://lukeshort.cloud/blog/kubernetes/#2021-05-18).
