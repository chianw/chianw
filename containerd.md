## Viewing pod information using crictl

I recently used containerd instead of Docker for the container runtime in a K8S cluster. I was familiar with the usual "docker ps", "docker logs" commands but was trying to figure out how to do the same with containerd. [crictl](https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md) came to the rescue

**Step 1** - Create a /etc/crictl.yaml file as follows on each node where you want to run crictl

<pre><code>runtime-endpoint: unix:///run/containerd/containerd.sock
image-endpoint: unix:///run/containerd/containerd.sock
timeout: 10
debug: false</code></pre>

**Step 2** - Run the commands to get the information you need

<pre><code>root@ubuntu2:~# crictl pods
POD ID              CREATED             STATE               NAME                                       NAMESPACE           ATTEMPT
1e69199276ccd       3 hours ago         Ready               mynginx                                    default             0
9bdd0e5a04754       4 hours ago         Ready               calico-kube-controllers-58497c65d5-ppdpz   kube-system         0
2b32c5bb71aeb       4 hours ago         Ready               coredns-78fcd69978-jkqms                   kube-system         0
8698fa90b2724       4 hours ago         Ready               coredns-78fcd69978-tzrnw                   kube-system         0
7d827c0ae7e64       4 hours ago         Ready               calico-node-2n7ht                          kube-system         0
b6766d83e24cb       5 hours ago         Ready               kube-proxy-pfr8m                           kube-system         0
root@ubuntu2:~#
root@ubuntu2:~# crictl ps
CONTAINER ID        IMAGE               CREATED             STATE               NAME                      ATTEMPT             POD ID
92af6b1b9f0ca       822b7ec2aaf21       3 hours ago         Running             mynginx                   0                   1e69199276ccd
8df2f2b5c4cab       76ba70f4748f9       4 hours ago         Running             calico-kube-controllers   0                   9bdd0e5a04754
d82b3a10ac951       8d147537fb7d1       4 hours ago         Running             coredns                   0                   2b32c5bb71aeb
a0e28c558d289       8d147537fb7d1       4 hours ago         Running             coredns                   0                   8698fa90b2724
dfa2e6791a67b       5ef66b403f4f0       4 hours ago         Running             calico-node               0                   7d827c0ae7e64
d0bd5cb1d520e       36c4ebbc9d979       5 hours ago         Running             kube-proxy                0                   b6766d83e24cb
root@ubuntu2:~#
root@ubuntu2:~#
root@ubuntu2:~# crictl logs 92af6b1b9f0ca
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2021/09/05 09:16:18 [notice] 1#1: using the "epoll" event method
2021/09/05 09:16:18 [notice] 1#1: nginx/1.21.1
2021/09/05 09:16:18 [notice] 1#1: built by gcc 8.3.0 (Debian 8.3.0-6)
2021/09/05 09:16:18 [notice] 1#1: OS: Linux 5.11.0-27-generic
2021/09/05 09:16:18 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2021/09/05 09:16:18 [notice] 1#1: start worker processes
2021/09/05 09:16:18 [notice] 1#1: start worker process 31
2021/09/05 09:16:18 [notice] 1#1: start worker process 32</code></pre>
