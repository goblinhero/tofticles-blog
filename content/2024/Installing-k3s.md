+++
title = 'Installing K3s'
date = 2024-03-12T09:49:23+01:00
draft = false
summary = 'It is this simple?!'
+++

I had been doing some reading on [Kubernetes](https://kubernetes.io/) and [Rancher](https://www.rancher.com/) and stumbled over the [K3S](https://k3s.io) project by the Rancher people. It's pretty much perfect for me, getting the full K8S in a nicely wrapped package where I don't actually need to understand all the moving parts of the full Kubernetes experience (and all the choices between competing choices within).

There is a number of ways to install K3S, but since I wanted to use Rancher on top of it (or beside it? Somewhere in the neighbourhood?), there are a few caveats due to the different versions.

## Prereqs

You'll need to have curl installed on all nodes as well as either disabling firewalls on the nodes or allowing [specific ports](https://docs.k3s.io/installation/requirements#networking). Since I'm using Ubuntu and having no public access - I went with disabling `ufw` entirely:

`sudo ufw disable`

## Extra parameters for the K3S install on the control plane node

#### INSTALL_K3S_VERSION

I took at look at Rancher's [support matrix](https://www.suse.com/suse-rancher/support-matrix/all-supported-versions/rancher-v2-8-2/) - and at the time of writing the 2.8.2 version supports K3S in version v1.27 while K3S latest is v1.28, so I had to supply an extra parameter for the K3S script - the `INSTALL_K3S_VERSION` parameter. K3S follows the Kubernetes versions with an extra version part for their own patches/hotfixes, so for the one I needed working with v1.27 of Kubernetes qua the Rancher support, I looked at the [releases](https://github.com/k3s-io/k3s/releases) for K3S for the latest patch - at the time of writing this is `v1.27.11+k3s1`. 

#### --cluster-init

This is optional, but allows for a later upgrade to a High Availability (HA) cluster (basically having at least 3 control plane nodes). For now, I'll skip this as I don't plan to go beyond one control plane nodes and two agent nodes

#### K3S_KUBECONFIG_MODE

The default mode for the kubeconfig is `600` which means only a root user can read the file. Since I plan to use Rancher on top, I'll change this to `644` to allow unpriviledged users to read it.

#### The resulting command to install the control plane of K3S

I ran this as a root user - not entirely sure it is needed, but I was not interested in fiddling with permissions and a half-installed K3S.

`curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.27.11+k3s1 K3S_KUBECONFIG_MODE="644" sh -s - server`

## Extra parameters for the K3S install on the agent nodes

#### INSTALL_K3S_VERSION

This needs to match the version of the master node - in my case `v1.27.11+k3s1`. 

#### K3S_URL

This needs to hit port 6443 on (one of) your control plane node(s). For my simple setup, simply the local IP-address of the control plane.

#### K3S_TOKEN

The token is created when installing the first control plane node and can be located with this simple command - simply copy the result for the K3S_TOKEN parameter:

`cat /var/lib/rancher/k3s/server/token`

#### The resulting command to install the agent nodes of K3s

`curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.27.11+k3s1 K3S_URL=192.168.0.71:6443 K3S_TOKEN=the-token-from-control-plane sh -`

And after doing that twice - once for each agent node, everything was working - I verified by issuing `kubectl get nodes` on the control plane node.

For something as complicated as Kubernetes, this is manageable even for a newbie such as myself (to both Linux and Kubernetes).