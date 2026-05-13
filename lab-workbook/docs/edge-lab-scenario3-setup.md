# Disaster Recovery Setup

Before getting started with this lab, we have a few items that need to be completed. This is a task overview.

-   Enable Nutanix Disaster Recovery on **Core** and **Cloud** Prism Central instances.
    -   This is a once-per-cluster operation.
    -   Seats 01, 11, 21, 31, and 41 will perform the **Core** cluster configuration.
    -   Seats 02, 12, 22, 32, and 42 will perform the **Cloud** cluster configuration.
-   Install Nutanix Guest Tools in your guest VM. Every seat will perform this operation.
    -   Nutanix Guest Tools are required for IP preservation during DR.
-   Apply a static IP Address to your Linux VM. Done by every seat.
    -   Use the PuTTY SSH client in your VDI session. q

## Enable Nutanix Disaster Recovery

follow along in the [guided DR enablement demo.](https://nutanix.storylane.io/share/85ff2hct9ydo?flow=5&scale=true) Come back to these instructions when you reach the end of the guided demo.

1.  Connect to the **Core** Prism Central as the local `admin` user.
    
2.  In the **Core** Prism Central, Click the settings button (gear icon) at the top-right corner of the web console.
    
3.  Click **Enable Disaster Recovery** in the Setup section on the left pane.
    
    -   The Nutanix Disaster Recovery dialog box runs pre-checks. If any pre-check fails, resolve the issue that is causing the failure and click **Check Again**. Ask your instructor for assistance if any pre-checks fail.

4.  Click **Enable** after all the pre-checks pass.


## Install NGT

1.  In the **Core** Prism Central, select **\> Compute & Storage > VMs**.
    
2.  Click the checkbox next to your **Linux-User##** VM, where ## is your assigned seat number, 1-50.
    
3.  Select **Actions**. Scroll down and select **Install NGT**
    
4.  The **Install NGT** pop-up box will appear. Select the radio button for **Restart as soon as the install is completed** then click **Confirm & Enter Password**
    
5.  On the right side enter the VM credentials of Username = `ubuntu` / Password = `nutanix/4u` and click **Done**.
    
6.  This installs NGT and reboots the VM. Verify NGT is installed by checking the NGT Status field in the Prism Central VM list view. It should say **Latest**
    

## Configure a Static IP for your VM Student

1.  In the **Core** Prism Central, select **\> Compute & Storage > VMs**.
    
2.  Select the checkbox next to your VM and click **\> Actions > Update**
    
3.  In the Configuration Tab, click **Next**
    
4.  In the Resources Tab, click the pencil where the network is "stretch-net"
    
5.  Change **Assignment Type** from **Assign Static IP** to **Assign with DHCP** and click **Save**
    

    !!! note

        **Assign with Static IP** in Prism is similar to a DHCP reservation and does not apply a static IP inside the guest VM. The guest VM still uses DHCP.

        We want the IP to persist after failover, therefore, we need to configure it as static inside the guest VM operating system.

6.  Log into your VM via SSH with Username = `ubuntu` / Password = `nutanix/4u`
    
7.  Copy the current network config file using the following command inside the VM SSH session.
    

    ```
    sudo cp /etc/netplan/50-cloud-init.yaml /etc/netplan/50-cloud-init.yaml.copy
    ```

8.  Record the adapter name and the IP Address where the current IP address is bound to after entering the following ip command.

    -   command

    ```
    ip a
    ```

    -   example output

    ```
    ubuntu@Linux-Tools-VM:~$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo

    2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> ... <---- Use this interface
        link/ether 50:6b:8d:e6:75:ac brd ff:ff:ff:ff:ff:ff
        altname enp0s3
        inet 10.42.231.50/25 ... scope global dynamic ens3 <-- Use this IP

    ```

9.  Open the network configuration file with nano

    ```
    sudo nano /etc/netplan/50-cloud-init.yaml
    ```

10.  Copy and paste from the example into the cloud-init.yaml configuration file. Change the **adapter name**, **IP Address** and **gateway** to match your assigned VM and cluster network information.

    -   template

    ```
    network:
      version: 2
      ethernets:
        ens3: # Replace with your interface name.
          dhcp4: false
          addresses:
            - 10.x.y.z/25 # Replace with your VM's assigned IP address. 
          routes:
            - to: 0.0.0.0/0
              via: 10.x.y.129 # Replace with your assigned stretch-net gateway.
    ```

    -   example

    ```
    network:
      ethernets:
        ens3:
          dhcp4: false
          addresses:
            - 10.55.89.201/25
          routes:
            - to: 0.0.0.0/0
              via: 10.55.89.129
      version: 2
    ```

11.  Apply the network configuration with the following command inside the guest VM.

    ```
    sudo netplan apply
    ```

12.  Verify that the VM can still reach the local network by performing a ping to the local gateway IP address.

    -   template

    ```
    ping 10.55.XX.128 # where XX is your network number
    ```

    -   example

    ```
    ping 10.55.89.129
    ```

## Next Steps

Now that our guest VM has a static IP address and can ping the local gateway, we're ready to perform our DR configuration and failover.

[← Back: Definitions](edge-lab-scenario3-def.md) | [Home](edge-getting-started.md) | [Next: Create Category & Associate VM →](edge-lab-scenario3-cat.md)