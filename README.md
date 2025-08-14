# Install Kubernetes

This script will install Kubernetes on Ubuntu 22.04.

## Stack 

* kubeadm
* containerd - *NOTE: is installed from binary download, not apt package*
* runc
* Kubernetes from Ubuntu
* Calico CNI

## Usage

### Setup Some Virtual Machines

Build at least two virtual machines, one for the control plane and one worker. Add more workers if you would like but this script will only be able to setup a single control plane node.

> NOTE: The script does not create the virtual machines (nodes). It only installs Kubernetes onto them. This means you can create the nodes in any way you want, but they must exist before running this script.

#### Suggested Node Sizes

Control Node
* 4G memory
* 40G disk
* 2 CPUs

Worker Nodes
* 8G memory
* 40G disk
* 4 CPUs

Combined Control Plane / Worker / Single Node "Cluster"
* 8G memory
* 40G disk
* 4 CPUs

## Install Kubernetes Onto the Nodes

Order of Operations

1. Build at least two virtual machines
1. Deploy the control plane node with this script
2. Configure the worker node with this script
3. Get the join command from the control plane node
4. Run that join command on the worker node(s) to join them to the Kubernetes cluster

### Control Plane Node

Use the `-c` option if the node is a control plane node.

Normally you would have one control plane node and `x` worker nodes.

On a control plane node run:

```
apt update && apt install git -y # optional if not already installed
git clone https://github.com/lftsang/install-kubernetes
cd install-kubernetes
# Note the "-c" switch here
./install-kubernetes.sh -c
```

### Worker Nodes

On a worker node run:

```
git clone https://github.com/lftsang/install-kubernetes
cd install-kubernetes
./install-kubernetes.sh
```

Then finally connect the worker node to the control plane node with the kubeadm command based on the output of the below which is run on the CP node.

```
kubeadm token create --print-join-command --ttl 0
```

Run the output of that command on the worker nodes.

### Single Node / Control Plane That Can Have Pods Scheduled On It

If you'd like a single node "cluster", ie. be able to schedule pods on the control plane, then run with the `-s` option.

```
./install-kubernetes.sh -s
```

This will untaint the control plane node so that pods can be scheduled on it.

## Thanks

This is based on the [Killer.sh CKS install script](https://github.com/killer-sh/cks-course-environment).
