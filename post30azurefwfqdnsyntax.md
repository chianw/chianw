## Azure Firewall Premium - syntax for FQDN filtering in Application Rules

Azure Firewall Rule Collections can be of type DNAT, Network or Application. In a Network Rule Collection, FQDN filtering is based on a static FQDN, for e.g. www.google.com but no wildcards like *.google.com . In Application Rule Collections, FQDN filtering supports wildcards like *.google.com 

This post aims to document the supported syntax when using FQDN filtering in Application Rule Collections. In the below Rule Collection Group, a DNAT rule collection is configured to allow SSH access into a Linux VM. There are 2 application rule collections - one that denies access to *.microsoft.com and another that allows everything else. TLS inspection is turned on in these rules.

![ruleset.png](https://github.com/chianw/chianw/blob/main/ruleset.png)



**FQDN filtering syntax to block *.microsoft.com**


![denyapplicationrule.png](https://github.com/chianw/chianw/blob/main/denyapplicationrule.png)


**FQDN filtering syntax to allow access to all other URLs**

![allowapplicationrule.png](https://github.com/chianw/chianw/blob/main/allowapplicationrule.png)


**Browing to https://windows.microsoft.com is blocked**

![blockresult.png](https://github.com/chianw/chianw/blob/main/blockresult.png)

**Browsing to https://ifconfig.me is allowed**

![allowresult.png](https://github.com/chianw/chianw/blob/main/allowresult.png)

