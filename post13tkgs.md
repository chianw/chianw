## Understanding the default DFW and GFW rules for a VMware TKGS cluster

vSphere with Tanzu (TKGS) can be deployed on vSphere Networking or on NSX-T networking. With NSX-T networking, a T1 logical router is automatically created for each vSphere namespace, behind which logical segments are attached. The K8S nodes are then connected to these logical segments. By default, there is a set of distributed firewall (DFW), gateway firewall (GFW) and NAT rules that are created to support this. In this post, the observed default DFW, GFW and NAT rules are documented for better understanding.


**Logical Diagram** - Shows the Supervisor Cluster Control VMs as well as a vSphere namespace call "red" behind which sits two TKGS K8S clusters

![tkgs1](https://github.com/chianw/chianw/blob/main/tkgs1.png)
