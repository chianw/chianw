## Understanding the default DFW and GFW rules for a VMware TKGS cluster

vSphere with Tanzu (TKGS) can be deployed on vSphere Networking or on NSX-T networking. With NSX-T networking, a T1 logical router is automatically created for each vSphere namespace, behind which logical segments are attached. The K8S nodes are then connected to these logical segments, with 1 K8S cluster per logical segment. By default, there is a set of distributed firewall (DFW), gateway firewall (GFW) and NAT rules that are created to support this. In this post, the observed default DFW, GFW and NAT rules are documented for better understanding.


**Logical Diagram** - Shows the Supervisor Cluster Control VMs as well as a vSphere namespace call "red" behind which sits two TKGS K8S clusters

![tkgs1](https://github.com/chianw/chianw/blob/main/tkgs1.png)


**Default DFW rules** - the default DFW rules for the "red" vSphere namespace containing 2 TKGS clusters. From these set of DFW rules for the "red" vSphere namespace, you can observe that:
1. all logical segments behind the T1 router for that namespace are allowed to communicate with one another
2. The Supervisor Control VM logical segment is allowed to communicate with all logical segments behind the T1 router
3. The entire Ingress CIDR + T1 LB uplink in the 100.64.x.x range is allowed to communicate to all logical segments behind the T1 router

In addition, there is a fixed set of default rules that allows ALL incoming traffic to reach the Supervisor Control VM logical segment, allows all egress traffic from all Tanzu logical segments behind T1 routers and blocks ingress traffic to all Tanzu logical segments behind T1 routers.

![tkgs1](https://github.com/chianw/chianw/blob/main/tkgs2.png)
