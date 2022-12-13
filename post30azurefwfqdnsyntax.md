## Azure Firewall - syntax for FQDN filtering in Application Rules

Azure Firewall Rule Collections can be of type DNAT, Network or Application. In a Network Rule Collection, FQDN filtering is based on a static FQDN, for e.g. www.google.com but no wildcards like \*.google.com . In Application Rule Collections, FQDN filtering supports wildcards like \*.google.com . Note that both Standard and Premium Firewall Policies support FQDN filtering in both Network Rules (static FQDN only) and Application Rules (wildcard FQDN support).

This post aims to document the supported syntax when using FQDN filtering in Application Rule Collections. In the below Rule Collection Group, a DNAT rule collection is configured to allow SSH access into a Linux VM. There are 2 application rule collections - one that denies access to \*.microsoft.com with priority 3500 and another that allows everything else with priority 4000. TLS inspection is turned on in these rules. Note that there are no Network Rule Collections in this Rule Collection Group. 

Note that in Azure Firewall, the rule collections with lower priority number gets processed first and the rules are terminating, which means firewall rule evaluation stops once there is a match.

![ruleset.png](https://github.com/chianw/chianw/blob/main/ruleset.png)



**FQDN filtering syntax to block \*.microsoft.com**


![denyapplicationrule.png](https://github.com/chianw/chianw/blob/main/denyapplicationrule.png)


**FQDN filtering syntax to allow access to all other URLs**

![allowapplicationrule.png](https://github.com/chianw/chianw/blob/main/allowapplicationrule.png)


**Browing to https://windows.microsoft.com is blocked**

![blockresult.png](https://github.com/chianw/chianw/blob/main/blockresult.png)

**Browsing to https://ifconfig.me is allowed**

![allowresult.png](https://github.com/chianw/chianw/blob/main/allowresult.png)

