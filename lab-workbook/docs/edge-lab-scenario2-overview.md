# Layer 2 Subnet Extension

![](images/slide-8.c0be60dc.jpg)

![](images/slide-9.20b63a5f.jpg)

In this section, you'll deploy a pair of network gateway VMs to facilitate layer 2 subnet extension between clusters at two sites, Core and Cloud. Thanks to subnet extension, VMs can seamlessly communicate between sites as if they were in the same network, and you can even move VMs between sites without changing their IP addresses.

The gateway VMs require a dedicated, reachable IP address on the primary VLAN subnet. This address is necessary to establish communication with the remote network gateway VM on the other cluster. For this lab, we will utilize the X.X.X.90/25 IP address in each cluster’s primary VLAN for the public address of the network gateway VMs.

These are the same two IP subnets used for the **Core** and **Cloud** Prism Central. Check your Connection Details launcher screen for details.

## Pair Availability Zones

Paired availability zones are a pre-requisite for using the Subnet Extension wizard between two clusters. We'll configure the AZ pairing between two Prism Central clusters here. This AZ pairing will be used by both Subnet Extension and our Disaster Recovery.

!!! info
    please follow along in this [guided AZ pairing demoopen in new window](https://nutanix.storylane.io/share/85ff2hct9ydo?flow=1&scale=true), but do not make changes in the cluster.

1.  In the **Core** Prism Central, select **\> Administration > Availability Zones**.
    
2.  Click **Connect to Availability Zone**.
    
3.  Click the drop-down for **Availability Zone Type** and select **Physical Location**.
    
4.  Enter the IP Address of the **Cloud** Prism Central.
    
5.  Enter your **username** `admin` and **password** and click **Connect**.
    

The **Core** and **Cloud** Availability Zones are now paired.

## Subnet Extension Logical Overview

The following diagram shows the L2 Subnet Extension between the **Core** and **Cloud** clusters. Network Gateway VMs are deployed with an external, reachable IP in the primary-core and primary-cloud subnets, shown on the left side of the gateway VM. Another interface on each network gateway VM extends into the stretch-net at both sites, shown on the right side of the gateway VM.

Note how the primary-core and primary-cloud IP subnet ranges are different, but the stretch-net IP subnet is the same at both sites! A VM in the **Core** stretch-net will have the same IP and reachability as a VM at the **Cloud** site once we're done!

![image](images/Subnet-Extend-Logical.2964b8f9.png)

!!! note
    In your lab environment these IP addresses will be different. Use the network ranges from your clusters assigned in connection details.

## Next Steps

Now that our AZs are paired, we're ready to deploy local and remote gateways to be used on both sides of our layer 2 subnet extension.

[← Back: VPN Connectivity](edge-lab-scenario1-vpn.md) | [Home](edge-getting-started.md) | [Next: Deploy Local Gateways →](edge-lab-scenario2-localgw.md)