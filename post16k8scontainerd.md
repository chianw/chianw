## Installing K8S with Containerd from scratch on Ubuntu 20.04

This post describes how you can install a K8S cluster from scratch on Ubuntu 20.04, using Containerd as the container runtime instead of Docker. Pre-requisites to this assumes that you have the Ubuntu 20.04 servers installed and running, and allowing SSH access. In my environment, I have 1 master and 2 worker nodes


**Step 1** Disable ufw and enable root ssh


<pre><code>sudo ufw disable
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo systemctl restart ssh</code></pre>
