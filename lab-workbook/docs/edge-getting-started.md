# Extending NCP from Core to Edge and Cloud

Instructor slide

![](images/slide-2.a5aa1e22.jpg)

Nutanix designed its software to give customers running workloads on cloud computing providers like Amazon Web Services (AWS) and Azure the same experience they expect from on-premises Nutanix clusters. Because Nutanix Cloud Clusters (NC2) runs Nutanix AOS and AHV with the same CLI, UI, and APIs, existing IT processes and third-party integrations continue to work regardless of where they run.

![](images/overview.99a1bcb0.png)

This lab is focused on NC2 on AWS. NC2 on AWS situates the complete Nutanix hyperconverged infrastructure (HCI) stack directly on an Amazon Elastic Compute Cloud (EC2) bare-metal instance. This bare-metal instance runs a Controller VM (CVM) and Nutanix AHV as the hypervisor just like any on-premises Nutanix deployment, using the AWS elastic network interface (ENI) to connect to the network. AHV guest VMs don't require any additional configuration to access AWS services or other EC2 instances.

After walking through the AWS configuration on a newly deployed cluster, we'll set up a layer 2 stretch, or Subnet Extension, from our private datacenter to the AWS cluster. The layer 2 stretch allows us to preserve a VM IP address and maintain site connectivity so we can fail over a single VM to AWS and ensure that our applications will work without breaking application dependencies. After we fail over, we will ensure we have connectivity across the layer 2 stretch.

![](images/gtslab.254a22fb.png)

By the end of this lab, you will be able to:

-   Understand how to set up the AWS components to connect your NC2 cluster to your private data center.
-   Create a layer 2 stretch (Subnet Extension) from a private data center to AWS for seamless application connectivity.
-   Create your own DR plan to move workloads into NC2 on AWS.

## Lab Agenda

### Setting up AWS After Deployment \[15min\]

-   Instructor-led view of AWS console

### VPN Connectivity \[5min\]

-   Routing in AWS
-   VPN Options

### Layer 2 Stretch Options and Configuration \[30min\]

-   Create local and remote gateways on-prem and in the cloud
-   Extend a subnet across two clusters.

### DR to AWS 2 While Using Layer 2 Stretch\[45min\]

-   Create protection policies and recovery plans to replicate VMs to the **Cloud**
-   Failover a VM to the **Cloud** and test the layer 2 stretch

[Next: Configure AWS →](edge-lab-scenario1-setupaws.md)