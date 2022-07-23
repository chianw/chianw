## Azure SQL Geo-replication

In this post, geo-replication of Azure SQL is being tested. An Azure SQL server is set up on Australia-East with 2 databases - geosqldb1 and TutorialDB. Only the TutorialDB database is replicated to another Azure SQL server in Japan East, but the geosqldb1 database is not replicated. 

> Note that replication is configured per-database. A SQL server can have multiple databases, and replication can be configured selectively for each database.


**1. Overview of setup - Azure SQL server and databases**

As setting up Azure SQL server is relatively straightforward, the step-by-step details of doing so are not shown for brevity. The below resources are all created in the same resource-group. The Azure SQL server in Australia East has 2 databases - geosqldb1 and TutorialDB. The Azure SQL server in Japan has the replicated instance of TutorialDB database, but not geosqldb1 which is not configured for geo-replication. To simplify testing, the database firewalls are configured to allow access from my public IP and basic user/password for SQL authentication is being used. 

![azuresqlreplica1.png](https://github.com/chianw/chianw/blob/main/azuresqlreplica1.png)

**2. Primary and Read-only replica**

As seen from the TutorialDB database in Australia East, it is the primary database with its read replica in Japan East

![azuresqlreplica2.png](https://github.com/chianw/chianw/blob/main/azuresqlreplica2.png)


**3. Peform SQL query against the TutorialDB database in Australia East**

Shows only 2 initial database entries in TutorialDB databse

![azuresqlreplica3.png](https://github.com/chianw/chianw/blob/main/azuresqlreplica3.png)

Insert in another record into TutorialDB database in Australia East

![azuresqlreplica4.png](https://github.com/chianw/chianw/blob/main/azuresqlreplica4.png)


**4. Read the replicated database in Japan**

The replicated database in Japan contains the updated table

![azuresqlreplica5.png](https://github.com/chianw/chianw/blob/main/azuresqlreplica5.png)



**5. VYOS interfaces and Network Security Groups**

The VYOS router has 2 interfaces - one in a subnet meant to access Internet and one to reach the Australia vnet, for the latter a static route needs to be configured as seen later. This is effectively 2-arm mode when it comes to NAT setup. Network Security Groups have allow all configured for simplified testing. 

![vyosinterface1](https://github.com/chianw/chianw/blob/main/vyosinterface1.png)

![vyosinterface2](https://github.com/chianw/chianw/blob/main/vyosinterface2.png)

![vyosnsg](https://github.com/chianw/chianw/blob/main/vyosnsg.png)


**6. IP Forwarding disabled for VYOS interfaces**  
Note that IP forwarding has to be disabled for VYOS interfaces in order for it to allow transit traffic

![vyosinterface1ipforward](https://github.com/chianw/chianw/blob/main/vyosinterface1ipforward.png)

![vyosinterface2ipforward](https://github.com/chianw/chianw/blob/main/vyosinterface2ipforward.png)

**7. Effective routes for VYOS interfaces**  

![vyosinterface1routes](https://github.com/chianw/chianw/blob/main/vyosinterface1routes.png)

![vyosinterface2routes](https://github.com/chianw/chianw/blob/main/vyosinterface2routes.png)

**8. Public IP used by VYOS for Internet access**  

Note that ICMP ping cannot be used by an Azure VM to check if it has Internet access, as ICMP from Azure vnet to external is not allowed. Curl will be a better way to verify Internet connectivity and here we perform a curl to https://ifconfig.me to find out the public IP used by VYOS for Internet access

![vyospublicip](https://github.com/chianw/chianw/blob/main/vyospublicip.png)

**9. Custom Route Table associated with Australia subnet and configured with default route pointing to VYOS interface of 10.0.1.4** 

![auroutetable](https://github.com/chianw/chianw/blob/main/auroutetable.png)

**10. Australia VM interface, effective routes and network security group**  

Here an allow-all rule has been added at the top of the Network Security Group to simplify access, but this is not a best practice. Notice the default route to VYOS from the custom route table overrides the original one. The subnet from Australia is associated with this custom route table.

![auvminterface1](https://github.com/chianw/chianw/blob/main/auvminterface1.png)

![auvmroutes](https://github.com/chianw/chianw/blob/main/auvmroutes.png)

![auvmnsg](https://github.com/chianw/chianw/blob/main/auvmnsg.png)


**11. Routing table, NAT configuration on VYOS**  

Notice a route for the Australia subnet of 10.1.0.0/24 has been added to VYOS pointing to the next-hop of the interface subnet for VYOS

![vyosroutes](https://github.com/chianw/chianw/blob/main/vyosroutes.png)


**12. Internet access from Australia VM**  

The VM in Australia is able to ping the VYOS interface which is its default gateway. Accessing ifconfig.me shows the same public IP which is seen when VYOS accesses ifconfig.me . This shows that the VM in Australia is using VYOS to access Internet.

![auvmpublicip](https://github.com/chianw/chianw/blob/main/auvmpublicip.png)


