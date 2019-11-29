---
layout: post
title: Running minikube in Fedora
---

To install and run minikube on [Fedora](https://getfedora.org/en/workstation/), we first need to install a Hypervisor. In the example below, I will be installing KVM by installing the virtualisation options available in Fedora (https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/). *Note: It is also possible to just install KVM: https://computingforgeeks.com/how-to-install-kvm-on-fedora/*.

## Fedora - Virtualisation

Fedora uses the libvirt family of tools as its virtualisation solution.

```bash
# Fedora virtualisation libraries
sudo dnf group install --with-optional virtualization

# Start and enable the kvm daemon
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```

The commands above install the virtualisation libraries required by minikube.

## minikube and kubectl

Now that KVM is installed in the system, I can proceed to download/install minikube and kubectl. Detailed instruction can be found here: minikube - https://kubernetes.io/docs/tasks/tools/install-minikube/ and kubectl - https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux.

```bash
# minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64   && chmod +x minikube
sudo mkdir -p /usr/local/bin/
sudo install minikube /usr/local/bin/

# kubectl
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl   && chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version
```

Now that the minikube and kubectl binaries are in the system, lets proceed to start minikube using `minikube start`.

If everything is setup correctly, running `kubectl get nodes` will return an output similar to the one below.

```bash
NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   15m   v1.16.2
```
