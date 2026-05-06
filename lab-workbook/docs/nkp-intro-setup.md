# NKP Advanced Hands-on Lab

# [#](#setup) Setup

![Environment](/cloudnative/assets/environment.b6d17c31.jpg)

1.  VDI to access the environment
    
2.  A 4-node HPOC cluster shared with up to 20 users
    
3.  Users authentication:
    
    -   Prism Central and staged NKP setup: adminuser## (ntnxlab.local domain)
    -   Admin VM: nutanix | nutanix/4u
4.  `Dedicated` compute resources for all the users to take the [Deploy NKP optional lab](/cloudnative/introduction/deploy-nkp/), with each user deploying:
    
    -   _Admin VM_
    -   NKP self-managed cluster
5.  `Shared` NKP Ultimate multi-cluster setup includes two pre-configured workload clusters, workload01 and workload02, allowing all users to proceed with the labs from [Deploy an app lab](/cloudnative/fundamentals/deploy-app/) without waiting for individual cluster deployments
    

Remember, all the details such as credentials, environment IPs, and other information is accessible on your bootcamp page.

Note

At a Nutanix event, certain IPs (STATIC in the table below) are pre-allocated for every user to do the labs.

#### [#](#for-getting-started-lab) For Getting started (LAB)

Component

IP

Subnet

IP Allocation

Example

Prism Central

#.#.#.**7**

primary

STATIC

10.38.30.7

Admin VM

\-

secondary

DHCP/IPAM

\-

NKP VMs

\-

secondary

DHCP/IPAM

\-

Important!

Your MetalLB "range" is just one IP. Since you won't be using this cluster, there is no need for additional IPs. You must still input the value on the format `#.#.#.<start>-#.#.#.<end>` even if it is a single IP. You can also find this information in the initial connection details page.

Take note of your user allocation from the list.

User

CONTROL\_PLANE\_ENDPOINT\_IP

LB\_IP\_RANGE

adminuser01

#.#.#.**134**

#.#.#.**135**\-#.#.#.**135**

adminuser02

#.#.#.**136**

#.#.#.**137**\-#.#.#.**137**

adminuser03

#.#.#.**138**

#.#.#.**139**\-#.#.#.**139**

adminuser04

#.#.#.**140**

#.#.#.**141**\-#.#.#.**141**

adminuser05

#.#.#.**142**

#.#.#.**143**\-#.#.#.**143**

adminuser06

#.#.#.**144**

#.#.#.**145**\-#.#.#.**145**

adminuser07

#.#.#.**146**

#.#.#.**147**\-#.#.#.**147**

adminuser08

#.#.#.**148**

#.#.#.**149**\-#.#.#.**149**

adminuser09

#.#.#.**150**

#.#.#.**151**\-#.#.#.**151**

adminuser10

#.#.#.**152**

#.#.#.**153**\-#.#.#.**153**

adminuser11

#.#.#.**154**

#.#.#.**155**\-#.#.#.**155**

adminuser12

#.#.#.**156**

#.#.#.**157**\-#.#.#.**157**

adminuser13

#.#.#.**158**

#.#.#.**159**\-#.#.#.**159**

adminuser14

#.#.#.**160**

#.#.#.**161**\-#.#.#.**161**

adminuser15

#.#.#.**162**

#.#.#.**163**\-#.#.#.**163**

adminuser16

#.#.#.**164**

#.#.#.**165**\-#.#.#.**165**

adminuser17

#.#.#.**166**

#.#.#.**167**\-#.#.#.**167**

adminuser18

#.#.#.**168**

#.#.#.**169**\-#.#.#.**169**

adminuser19

#.#.#.**170**

#.#.#.**171**\-#.#.#.**171**

adminuser20

#.#.#.**172**

#.#.#.**173**\-#.#.#.**173**

#### [#](#for-the-rest-of-labs) For the rest of labs

Starting from the [Deploy an app (LAB)](/cloudnative/fundamentals/deploy-app/) use this section.

Component

IP

Example

NKP Management UI

#.#.#.**16**

https://10.38.30.16/dkp/kommander/dashboard

NKP Workload 01 Ingress

#.#.#.**19**

10.38.30.19

NKP Workload 02 Ingress

#.#.#.**22**

10.38.30.22