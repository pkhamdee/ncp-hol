# Untitled Page

# [#](#create-a-migration-plan) Create a Migration Plan

Once the source and destination are set up, create the migration plan.

1.  Click `Create a Migration Plan`
    
    !!! note    
        If Create a Migration Plan button is greyed out, wait 2-3 minutes.
    
    ![](/images/vm-migration-plan1.4764a70a.png)
    
2.  Enter a name for the Plan `User##-VM-Plan`. Click `Proceed`
    
    ![](/images/vm-migration-plan2.d8c15dbb.png)
    
3.  Select your Source AHV and Target AHV environments. Click `Next`
    
    -   **Select a Source** : `Nutanix AHV Source or the name used before`
    -   **Select a Target** : `Nutanix AHV Target or the name used before`
    -   **Target Project** : `None`
    -   **Target Cluster** : `Select the destination AHV cluster`
    -   **Target Container** : `default`
    
    ![](/images/vm-migration-plan3.cd692d50.png)
    
4.  Select your Linux VM to move, `XXX-User##` where `XXX` is the preface provided to you by your instructor. Once selected, click `Next` in the bottom right-hand corner.
    
    ![](/images/vm-migration-plan4b.29e3733e.png)
    
    Please ignore any warning signs when adding the VM. This will not affect the migration.
    
    ![](/images/vm-migration-plan5b.aad08de3.png)
    
5.  For Networks, Select `primary` for the Target Network. Click `Next`
    
    ![](/images/vm-migration-plan6b.4132b859.png)
    
6.  On the VM preparation page, select the options shown and provide the username and password for the Linux VM.
    
    -   **Preparation Mode** - `Automatic`. Manual mode only migrates data over. VM preparation script and all OS configuration options will have to be run manually in manual mode.
    -   **Guest Operations**
        -   `DHCP` and make sure `Install Nutanix guest Tools(NGT) on target VMs` is **unchecked**. If DHCP is used, like in our case, VMs will get a new IP address on the destination._Retain static IP Addresses from source VMs_: If VMs have static IPs, they are retained on the destination.
        -   _Uninstall Virt Tools from other Hypervisor on target VMs_: Since we are moving to AHV, we do not need non AHV Tools installed on the VM.
        -   _Bypass guest operations on source VMs_: Will only do data migration and no guest OS operations. **Keep this unchecked.**
    -   **Credentials**
        -   _User Name_: `rocky`
        -   _password_: `nutanix/4u`
    
    ![](/images/vm-migration-plan7.fbc25e07.png)
    
    !!! note 
        You only need to enter these credentials into the **Linux VMs** field and can leave the Windows credentials empty.
    
7.  On the **VM Settings** page, keep the defaults and select `Next`. The fields allow for individual VM settings to be modified. This includes settings like enabling Memory Overcommit or applying categories that are present in Prism Central. You can learn more about categories in the advanced migration section. If you want to learn more about the different fields, refer to the [Move Guide](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Move-v5_5:top-create-migration-plan-t.html)
    
    ![](/images/vm-migration-plan8.4d40f0d6.png)
    
8.  On the **Summary** page, Click `Save and Start`. Selecting Save only saves the plan. Save and start will save the plan and start the Migration.
    
    -   Note: If you get a migration error upon saving the plan, wait around 30 seconds and try again.
    
    ![](/images/vm-migration-plan9b.e8f1b999.png)