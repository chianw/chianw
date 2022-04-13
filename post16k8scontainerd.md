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




