## Installing K8S with Containerd from scratch on Ubuntu 20.04

This post describes how you can install a K8S cluster from scratch on Ubuntu 20.04, using Containerd as the container runtime instead of Docker. Pre-requisites to this assumes that you have the Ubuntu 20.04 servers installed and running, and allowing SSH access. In my environment, I have 1 master and 1 worker nodes


**Step 1** Disable ufw and enable root ssh


```
sudo ufw disable
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo systemctl restart ssh
```

```
## set root password, by default root user has no password in Ubuntu preventing SSH login
sudo passwd
```

**Step 2** Ensure host file is updated with cluster node FQDN and IP on all hosts

Edit /etc/hosts file to add A record for all node IP and their hostnames. Repeat on each node within the K8S cluster

```
root@napp-k8s2:~# cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 napp-k8s2
192.168.110.70 napp-k8s1
192.168.110.71 napp-k8s2
```

**Step 3** Letting iptables see bridged traffic on all hosts

```
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```

**Step 4** Disable swap on all hosts - please recheck /etc/fstab after this to ensure swap disabled, note that the below does not seem to remote swap in /etc/fstab so you may have to manually do it!

```
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
sudo swapoff -a
```

**Step 5** Install and restart Containerd on all hosts in the K8S cluster

```
# Configure persistent loading of modules
sudo tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF

# Load at runtime
sudo modprobe overlay
sudo modprobe br_netfilter


# Ensure sysctl params are set
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

# Reload configs
sudo sysctl --system

# Install required packages
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates

# Add Docker repo
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install containerd
sudo apt update
sudo apt install -y containerd.io

# Configure containerd and start service
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml


# restart containerd
sudo systemctl restart containerd
sudo systemctl enable containerd
systemctl status  containerd

```

**Step 6** Edit /etc/containerd/config.toml on each host to make use of SystemdCgroup, take note of the indentation

```
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true

# restart containerd
sudo systemctl restart containerd

```

**Step 7** Installing kubeadm , kubelet and kubectl on all hosts

```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl

sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg


echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main"| sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

## install a specific version of k8s
apt-get install -y kubelet=1.21.9-00 kubeadm=1.21.9-00 kubectl=1.21.9-00
apt-mark hold kubelet kubeadm kubectl

# enable kubelet
systemctl enable kubelet

```




