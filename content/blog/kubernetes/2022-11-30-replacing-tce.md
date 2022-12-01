---
title: 2022-11-30
date: 2022-11-30 12:00:00
categories:
  - kubernetes
tags:
  - docker
  - kubernetes
  - tanzu
  - tkg
  - tkgm
---

[Back to Kubernetes blog.](../#kubernetes)

# Replacing Tanzu Community Edition (TCE)

{{< image src="../../images/blog/kubernetes/2022-11-30-replacing-tce.jpg" alt="Group of developers programming" set="fit" >}}

Tanzu Communiy Edition (TCE) is [dead](https://tanzucommunityedition.io/). Long live TCE! It was a bundle of free and open source applications (that will still always remain free and open source!). This was a community project lead by VMware (full disclaimer: I currently work there) of what is collectively known as Tanzu Kubernetes Grid multicloud (TKGm) and Tanzu Application Platform (TAP). The most common use-case for TCE was for evaluation or local development purposes.

Users can now get TKGm for [free](https://www.vmware.com/go/get-tkg) to use for personal use. A Kubernetes deployment of up to 100 cores is allowed through this trial license. One of the reasons behind this is to make migration seamless as TCE was a radically different codebase. For example, you probably do not want to use Red Hat's AWX (open source) over Ansible Tower (product) due to the quality of code and support. In many cases, you are looking at a painful migration.

For local deveopment on your workstation, the official recommendation is to use Minikube or KinD. I used to use TCE for lab environments and demos (both internal and external). It's great for getting a quick and dirty Kubernetes cluster up. So what is a person to do in a world without TCE? Let's dive into what TCE provides from a Kubernetes platform standpoint and then see if we can re-create it.

- Install TCE.

```
$ wget https://github.com/vmware-tanzu/community-edition/releases/download/v0.12.1/tce-linux-amd64-v0.12.1.tar.gz
$ tar -xvf tce-linux-amd64-v0.12.1.tar.gz
$ cd tce-linux-amd64-v0.12.1
$ bash install.sh
```

- View the default package plugins.

```
$ tanzu plugin list
  NAME                DESCRIPTION                                                        SCOPE       DISCOVERY      VERSION  STATUS     
  apps                Applications on Kubernetes                                         Standalone  default-local  v0.6.0   installed  
  builder             Build Tanzu components                                             Standalone  default-local  v0.11.4  installed  
  cluster             Kubernetes cluster operations                                      Standalone  default-local  v0.11.4  installed  
  codegen             Tanzu code generation tool                                         Standalone  default-local  v0.11.4  installed  
  conformance         Run Sonobuoy conformance tests against clusters                    Standalone  default-local  v0.12.1  installed  
  diagnostics         Cluster diagnostics                                                Standalone  default-local  v0.12.1  installed  
  kubernetes-release  Kubernetes release operations                                      Standalone  default-local  v0.11.4  installed  
  login               Login to the platform                                              Standalone  default-local  v0.11.4  installed  
  management-cluster  Kubernetes management cluster operations                           Standalone  default-local  v0.11.4  installed  
  package             Tanzu package management                                           Standalone  default-local  v0.11.4  installed  
  pinniped-auth       Pinniped authentication operations (usually not directly invoked)  Standalone  default-local  v0.11.4  installed  
  secret              Tanzu secret management                                            Standalone  default-local  v0.11.4  installed  
  unmanaged-cluster   Deploy and manage single-node, static, Tanzu clusters.             Standalone  default-local  v0.12.1  installed
```

- Create a Kubernetes workload cluster.

```
$ tanzu unmanaged-cluster create lukeshortcloud
```

- You will automatically be placed into the the new context. Notice how this is a kind cluster. It is using kind for the actual deployment of Kubernetes. More specifically, kind is using the [Cluster API Provider for Docker (CAPD)](https://github.com/kubernetes-sigs/cluster-api/tree/main/test/infrastructure/docker).

```
$ kubectl config get-contexts
CURRENT   NAME                  CLUSTER               AUTHINFO              NAMESPACE
*         kind-lukeshortcloud   kind-lukeshortcloud   kind-lukeshortcloud
$ sudo docker ps -a
CONTAINER ID   IMAGE                                           COMMAND                  CREATED         STATUS         PORTS                       NAMES
4d9cdfdd917e   projects.registry.vmware.com/tce/kind:v1.22.7   "/usr/local/bin/entr…"   2 minutes ago   Up 2 minutes   127.0.0.1:38359->6443/tcp   lukeshortcloud-control-plane
```

- What is pre-installed? The only third-party components installed are (1) Calico for CNI and (2) kapp-controller for installing Carvel packages (a more advanced take on package management compared to Helm charts).

```
$ kubectl get pods --all-namespaces
NAMESPACE            NAME                                                   READY   STATUS     RESTARTS   AGE
kube-system          calico-kube-controllers-75b5f998f9-5tmmf               0/1     Pending    0          61s
kube-system          calico-node-vq5rl                                      0/1     Init:0/3   0          61s
kube-system          coredns-78fcd69978-gsf6r                               0/1     Pending    0          2m19s
kube-system          coredns-78fcd69978-qhrc8                               0/1     Pending    0          2m19s
kube-system          etcd-lukeshortcloud-control-plane                      1/1     Running    0          2m35s
kube-system          kube-apiserver-lukeshortcloud-control-plane            1/1     Running    0          2m33s
kube-system          kube-controller-manager-lukeshortcloud-control-plane   1/1     Running    0          2m34s
kube-system          kube-proxy-ng46j                                       1/1     Running    0          2m20s
kube-system          kube-scheduler-lukeshortcloud-control-plane            1/1     Running    0          2m33s
local-path-storage   local-path-provisioner-74567d47b4-7lcwf                0/1     Pending    0          2m19s
tkg-system           kapp-controller-779d9777dc-9vx47                       1/1     Running    0          2m19s
```

- Let’s look at the packages we have available to install. These are a combination of open source equivilanets of what is provided both in the Tanzu Standard repository and the Tanzu Application Platform (TAP) repository.

```
$ tanzu package available list
  NAME                                                    DISPLAY-NAME                 SHORT-DESCRIPTION                                                                                                                                          LATEST-VERSION  
  app-toolkit.community.tanzu.vmware.com                  App-Toolkit package for TCE  Kubernetes-native toolkit to support application lifecycle                                                                                                 0.2.0           
  cartographer-catalog.community.tanzu.vmware.com         Cartographer Catalog         Reusable Cartographer blueprints                                                                                                                           0.3.0           
  cartographer.community.tanzu.vmware.com                 Cartographer                 Kubernetes native Supply Chain Choreographer.                                                                                                              0.3.0           
  cert-injection-webhook.community.tanzu.vmware.com       cert-injection-webhook       The Cert Injection Webhook injects CA certificates and proxy environment variables into pods                                                               0.1.1           
  cert-manager.community.tanzu.vmware.com                 cert-manager                 Certificate management                                                                                                                                     1.8.0           
  contour.community.tanzu.vmware.com                      contour                      An ingress controller                                                                                                                                      1.20.1          
  external-dns.community.tanzu.vmware.com                 external-dns                 This package provides DNS synchronization functionality.                                                                                                   0.10.0          
  fluent-bit.community.tanzu.vmware.com                   fluent-bit                   Fluent Bit is a fast Log Processor and Forwarder                                                                                                           1.7.5           
  fluxcd-source-controller.community.tanzu.vmware.com     Flux Source Controller       The source-controller is a Kubernetes operator, specialised in artifacts acquisition from external sources such as Git, Helm repositories and S3 buckets.  0.21.5          
  gatekeeper.community.tanzu.vmware.com                   gatekeeper                   policy management                                                                                                                                          3.7.1           
  grafana.community.tanzu.vmware.com                      grafana                      Visualization and analytics software                                                                                                                       7.5.11          
  harbor.community.tanzu.vmware.com                       harbor                       OCI Registry                                                                                                                                               2.4.2           
  helm-controller.fluxcd.community.tanzu.vmware.com       Flux Helm Controller         The Helm Controller is a Kubernetes operator, allowing one to declaratively manage Helm chart releases with Kubernetes manifests.                          0.17.2          
  knative-serving.community.tanzu.vmware.com              knative-serving              Knative Serving builds on Kubernetes to support deploying and serving of applications and functions as serverless containers                               1.0.0           
  kpack-dependencies.community.tanzu.vmware.com           kpack dependencies           Dependencies in the form of Buildpacks and Stacks for the kpack package                                                                                    0.0.27          
  kpack.community.tanzu.vmware.com                        kpack                        kpack builds application source code into OCI compliant images using Cloud Native Buildpacks                                                               0.5.3           
  kustomize-controller.fluxcd.community.tanzu.vmware.com  Flux Kustomize Controller    Kustomize controller is one of the components in GitOps toolkit.                                                                                           0.21.1          
  local-path-storage.community.tanzu.vmware.com           local-path-storage           This package provides local path node storage and primarily supports RWO AccessMode.                                                                       0.0.22          
  multus-cni.community.tanzu.vmware.com                   multus-cni                   This package provides the ability for enabling attaching multiple network interfaces to pods in Kubernetes                                                 3.8.0           
  prometheus.community.tanzu.vmware.com                   prometheus                   A time series database for your metrics                                                                                                                    2.27.0-1        
  velero.community.tanzu.vmware.com                       velero                       Disaster recovery capabilities                                                                                                                             1.8.0           
  whereabouts.community.tanzu.vmware.com                  whereabouts                  A CNI IPAM plugin that assigns IP addresses cluster-wide                                                                                                   0.5.1 
```

Great! Now we know what makes Tanzu tick. Let's clean up what we have and try to re-create this with KinD.

- Let’s clean up and replace TCE.

```
$ tanzu unmanaged-cluster delete lukeshortcloud
```

- Install kind:

```
$ GO111MODULE="on" go get sigs.k8s.io/kind@v0.17.0
$ sudo cp ~/go/bin/kind /usr/local/bin/
```

- Deploy a local kind cluster using CAPD:

```
$ kind create cluster --image kindest/node:v1.22.7 --name lukeshortcloud
```

- The context is automatically changed:

```
$ kubectl config get-contexts
CURRENT   NAME                  CLUSTER               AUTHINFO              NAMESPACE
*         kind-lukeshortcloud   kind-lukeshortcloud   kind-lukeshortcloud
```

- How do the Pods compare here? For one thing, the CNI plugin is [kindnet](https://github.com/aojea/kindnet) instead of Calico. We are also missing kapp-controller.

```
$ kubectl get pods --all-namespaces
NAMESPACE            NAME                                                   READY   STATUS    RESTARTS   AGE
kube-system          coredns-78fcd69978-h5vg8                               1/1     Running   0          36s
kube-system          coredns-78fcd69978-qqgh6                               1/1     Running   0          36s
kube-system          etcd-lukeshortcloud-control-plane                      1/1     Running   0          51s
kube-system          kindnet-4x2bv                                          1/1     Running   0          37s
kube-system          kube-apiserver-lukeshortcloud-control-plane            1/1     Running   0          51s
kube-system          kube-controller-manager-lukeshortcloud-control-plane   1/1     Running   0          52s
kube-system          kube-proxy-q9wlk                                       1/1     Running   0          37s
kube-system          kube-scheduler-lukeshortcloud-control-plane            1/1     Running   0          49s
local-path-storage   local-path-provisioner-74567d47b4-d7fwg                1/1     Running   0          36s
```

- Let's redeploy with Calico on kind instead.

```
$ cat <<EOF | kind create cluster --name lukeshortcloud --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  disableDefaultCNI: true
  podSubnet: 192.168.0.0/16
EOF
$ kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

- The last thing we need for the infrastructure is to install kapp-controller. This is part of the suite of Carvel tools that provide more advanced packaging combared to Helm.

```
$ kubectl apply -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
```

- What pods do we have now? Ah, that's better. It's just like TCE!

```
$ kubectl get pods --all-namespaces
NAMESPACE            NAME                                                   READY   STATUS    RESTARTS   AGE
kapp-controller      kapp-controller-57f6bf9f96-w7rnn                       2/2     Running   0          74s
kube-system          calico-kube-controllers-798cc86c47-6kf2d               1/1     Running   0          2m25s
kube-system          calico-node-ghrhk                                      1/1     Running   0          2m25s
kube-system          coredns-565d847f94-mrv9s                               1/1     Running   0          5m46s
kube-system          coredns-565d847f94-wfsdj                               1/1     Running   0          5m46s
kube-system          etcd-lukeshortcloud-control-plane                      1/1     Running   0          6m
kube-system          kube-apiserver-lukeshortcloud-control-plane            1/1     Running   0          6m
kube-system          kube-controller-manager-lukeshortcloud-control-plane   1/1     Running   0          6m
kube-system          kube-proxy-nv2sj                                       1/1     Running   0          5m46s
kube-system          kube-scheduler-lukeshortcloud-control-plane            1/1     Running   0          6m
local-path-storage   local-path-provisioner-684f458cdd-5k27h                1/1     Running   0          5m46s
```

That's it! We now have a Kubernetes lab environment similar to what TCE provides. What about those fancy packages? Keeping aligned with TCE, you can [install TAP](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.3/tap/GUID-install.html) which deserves its own separate blog. Otherwise, [Bitnami Helm charts](https://bitnami.com/stacks/helm) provide a large catalogue of useful applications with no strings attached.

Does this help you with your Kubernetes adoption journey? Looking for more Kubernetes advice? Feel free to [call me, beep me, if you want to reach me](../../#contact)!
