## Installing K8S with Containerd from scratch on Ubuntu 20.04

This post describes how you can install a K8S cluster from scratch on Ubuntu 20.04, using Containerd as the container runtime instead of Docker. Pre-requisites to this assumes that you have the Ubuntu 20.04 servers installed and running, and allowing SSH access. In my environment, I have 1 master and 2 worker nodes


**Step 1** Disable ufw and enable root ssh


<pre><code>sudo ufw disable
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo systemctl restart ssh</code></pre>

<pre><code>## set root password, by default root user has no password in Ubuntu preventing SSH login
sudo passwd</code></pre>

**Step 2** Ensure host file is updated with cluster node FQDN and IP on all hosts

Edit /etc/hosts file to add A record for all node IP and their hostnames. Repeat on each node within the K8S cluster

<pre><code>root@napp-k8s2:~# cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 napp-k8s2
192.168.110.70 napp-k8s1
192.168.110.71 napp-k8s2</code></pre>
