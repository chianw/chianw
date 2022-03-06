## Useful Antrea commands

With NSX-T 3.2 release, it is now capable of integrating with Antrea via "Antrea NSX Adaptor". The Antrea NSX Adaptor runs as a pod and helps in communication between NSX Manager and kube-api-server. This allows ClusterNetworkPolicies to be created as DFW policies in NSX which are then applied to the K8S cluster where Antrea is running. 

This post aims to provide some useful commands to view Antrea objects on the K8S cluster

**Installation and integration steps**
The architecture of Antrea with NSX-T and the installation steps are well documented [here](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/administration/GUID-9197EF8A-7998-4D1B-B968-067007C56B5C.html) and hence will not be repeated.




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
