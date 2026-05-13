# Failover

!!! note

    In the event of a true disaster, the Primary, or **Core** Prism Central will be offline, therefore a recovery operation must be initiated on the target Prism Central, named **Cloud** in this case.

1.  In the **Core** Prism Central, login as local `admin` and select **\> Compute & Storage > VMs**, take note of your VM's IP Address.
    
2.  In the **Cloud** Prism Central, login as local `admin`, select **\> Data Protection > Recovery Plans**.
    
3.  Select the check box next to your recovery plan, **RecoveryPlan-##** where ## is your user number.
    
4.  Click **Actions**, then click **Validate**.
    
    ![image](images/validate_rp.76339b54.png)
    
5.  Click **Proceed**
    
6.  If your Recovery Plan validates successfully, you will see **Validation completed successfully**
    
7.  Click **Close**
    
8.  Select the check box next to your Recovery Plan **RecoveryPlan-##** if not already checked and select **\> Actions > Failover**.
    
9.  If this were a real disaster, in the Failover Type area, you would select **Unplanned Failover**, but because this is a lab, we will select **Planned Failover**
    
10.  Verify in the **Recovery Status** portion that you see **Total 1 Entities**. This should be your single VM entity selected by the plan.
    
    ![image](images/failover_from_rp.d124c423.png)
    
11.  Click **Failover**.
    
12.  In the pop-up box for **Planned Failover**, type **Failover** in the text box and click the **Failover** button.
    
    ![image](images/failover.9f137fb8.png)
    
13.  This operations will take approximately three minutes. Then your VM will be running on the **Cloud** cluster.
    
14.  Once your VM is up, log in with the VM credentials of Username = `ubuntu` / Password = `nutanix/4u`. Use the VM console screen in the **Cloud** Prism Central.
    
15.  Ping 10.x.y.129 (Core Gateway). This will validate that traffic is going through the L2 Stretch and out of the gateway at the **Core** site.
    
16.  Ping 8.8.8.8 (Google DNS). This will validate traffic is going from the **Cloud** cluster, over the Subnet Extension, and out from **Core** cluster to the internet.
    

## Key Takeaways

Now that you've completed these lab exercises you can deploy and fail workloads over anywhere all while maintaining connectivity. To recap, here's what we learned:

-   How to configure cloud networking in AWS and pairing AWS networks to on-prem networks via VPN.
-   How to set up a layer 2 stretch by deploying Nutanix gateways and creating a subnet extension between them.
-   The constructs of Nutanix Disaster Recovery and how to configure a **category**, **protection policy** and **recovery plan**.
-   How to complete a planned failover of a VM to the **Cloud** cluster while retaining both its IP address and connectivity to existing on-prem workloads.
-   How to extend to the cloud! 🪜 ☁️

[← Back: Create a Recovery Plan](edge-lab-scenario3-recover.md) | [Home](edge-getting-started.md) | [Next: Conclusion →](edge-conclusion.md)