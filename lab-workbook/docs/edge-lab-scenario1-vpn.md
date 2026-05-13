# VPN Connectivity

## Routing in AWS

In AWS, the route table can only set destinations to an elastic network interface (ENI). The VPN gateway in the earlier portion of the lab showed how we directed our AWS traffic to our private data center. Today creating a user VM as a VPN or firewall on an NC2 AWS cluster is not supported. This is partially due to the above routing limitation and how NC2 handles source and destination MAC addresses between nodes.

![The destination MAC matches and the secondary IP is present so traffic proceeds.](images/proccedtraffic1.b25b7a2c.png)

In the above example, traffic going from User VM1 on node 1 to User VM2 on node 2 is accepted because when it reaches the ENI on node 2 with the correct MAC address, it finds a match with the secondary IP. Therefore traffic will proceed.

![The destination MAC matches but the secondary IP is not matched so traffic is dropped.](images/denytraffic2.cf657603.png)

However, in the above example, User VM2 is a Network Virtual Appliance (NVA) acting as a VPN or router. User VM1 wants to send traffic to a private data center. The destination IP is 10.19.160.10 for the private data center resource. This IP does not exist on any ENI. If you try to use User VM2 as the router or VPN, AWS will send traffic to the ENI of User VM2 but it will be dropped as the destination IP doesn't appear. Nutanix is working on a fix for this but as of today, your only options are the AWS VPN or deploying the NVA as a native EC2 instance. The native EC2 instance can be deployed using AWS ENIs for the private and public interfaces. The ENIs from the NVA can be listed as the destination in the AWS routing table.

## On-premises Connectivity

There are a lot of options for connecting your AWS VPC to a private data center. Below are some available options:

-   AWS Site-to-Site VPN
-   Direct Connect
-   AWS Transit Gateway + AWS VPN
-   Software Network Virtual Appliance

This lab will focus on the AWS Site-to-Site VPN and using a Software Network Virtual Appliance (NVA). These two options tend to be the most common when setting up a proof-of-concept.

**AWS Site-to-Site VPN**

![AWS VPN attached to the AWS VPC](images/vpn3.9c21a4c6.png)

The AWS VPN is created by supplying a customer gateway, representing the public IP of your private data center VPN. Once the VPN is created and configured, you can attach it to your AWS VPC. Once the VPN is attached, it will show as a destination option in the AWS route table. Using the example in the diagram above, we would edit the AWS private route table and set the destination for all private data center traffic to the AWS VPN Gateway connected to on-prem.

**Network Virtual Appliance**

![Network Virtual Appliance using ENIs](images/nva4.64f47dfe.png)

To use an NVA, deploy EC2 instances that meet the vendor's requirements in the same VPC as your NC2 cluster. After deploying the EC2 instance, add ENIs for network interfaces. These ENIs would then be able to appear as options in the AWS route table. Generally, customers tend to prefer to use the same VPN vendor that they are using in their private data center, but this isn't a mandatory requirement.

Once connectivity is established to the private data center, you just have to ensure that the NC2 Clusters security groups are edited to allow traffic in. In the earlier portion of the lab, we used a Custom VPC Security Group to allow all on-premises traffic into the cluster.

## Takeaways

-   Nutanix native networking integration allows a lot of flexibility for the customer in deciding on how they want to connect.
-   After connecting your VPN to your private data center you may still have to edit the NC2 cluster's security groups.

[← Back: Configure AWS](edge-lab-scenario1-setupaws.md) | [Home](edge-getting-started.md) | [Next: Overview & Pre-reqs →](edge-lab-scenario2-overview.md)