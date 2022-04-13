## Importing Antrea images with containerd

I have a vanilla K8S cluster v1.21.9 that is newly built and using Containerd and not Docker. CNI has not been installed yet and this post aims to describe some of the steps to import Antrea CNI image into the K8S nodes before installing them. Normally this is done via "docker import" but since Docker is not used, the alternative using "ctr" command is explained here.

**Installation and integration steps**

Download the below 2 files from Vmware, the first file is required to install basic Antrea CNI, the second file is to inter-operate with NSXT. As of time of writing in April 2022, the below files are the latest available.


antrea-advanced-1.2.3+vmware.3.19009828.zip

antrea-interworking-0.2.0.zip


**View available Antrea Cluster Network Policies or ancp** - Run the following command

<pre><code>root@ubuntu-jump:~/antrea# kubectl get acnp
NAME                                   TIER                       PRIORITY             DESIRED NODES   CURRENT NODES   AGE
allow-all-pods-in-namespace            application                10                   3               3               6s
f3c6e3a8-a016-4344-8324-37f4bd632f74   nsx-category-application   1.0000000532907096   3               3               45m</code></pre>


**View available tiers in Antrea** - Those prefixed with "nsx-category" are pushed from NSX. Antrea itself has a set of policy tiers built-in. The priority number determines the order of policy evaluation, with the lower number resulting in the tier being evaluated first.

<pre><code>root@ubuntu-jump:~/antrea# kubectl get tiers
NAME                          PRIORITY   AGE
application                   250        3d
baseline                      253        3d
emergency                     50         3d
networkops                    150        3d
nsx-category-application      4          3d
nsx-category-emergency        1          3d
nsx-category-environment      3          3d
nsx-category-ethernet         0          3d
nsx-category-infrastructure   2          3d
platform                      200        3d
securityops                   100        3d</code></pre>


**View available ClusterGroups in Antrea** - Run the following command.

<pre><code>root@ubuntu-jump:~/antrea# kubectl get cg
NAME                                     AGE
9dfc4e04-2d17-40d1-afcf-250b0d191c62     44m
9dfc4e04-2d17-40d1-afcf-250b0d191c62-0   44m</code></pre>
