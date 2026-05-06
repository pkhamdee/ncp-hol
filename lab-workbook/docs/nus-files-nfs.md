# Nutanix Unified Storage

# [#](#files-create-nfs-export) Files: Create NFS Export

## [#](#overview) Overview

In this exercise, you will create and test an NFSv4 export used to support clustered applications, store application data such as logging, or store other unstructured file data commonly accessed by Linux clients.

## [#](#using-nfs-exports) Using NFS Exports

### [#](#enabling-nfs-protocol) Enabling NFS Protocol

One-time operation

Enabling the NFS protocol needs to be performed once per Files server. It may have already been completed in your environment. If NFS is enabled, proceed to [Creating the Export](#creating-the-export).

1.  Within your Nutanix Files browser tab, navigate to **Configuration > Authentication > Directory Services**.
    
2.  Click **Show NFS Advanced Options**.
    
3.  Uncheck **Enable NFSv3 by default for all exports** as we will only test NFS4 exports in this lab.
    
4.  Click on **Update**.
    
    ![](/unified-storage/assets/1.05824aea.png)
    

### [#](#creating-the-export) Creating The Export

1.  Select **Shares** and click on **Create a New Share**
    
2.  Fill out the following fields, and click **Next**.
    
    -   **Name** - `User##`\-logs
    -   **Description (Optional)** - File share for system logs
    -   **Share Path (Optional)** - Leave blank
    -   **Max Size (Optional)** - Leave blank
    -   **Select Protocol** - NFS
    
    ![](/unified-storage/assets/2.50752892.png)
    
3.  Fill out the following fields, and click **Next**.
    
    -   Select **Enable Self Service Restore**. These snapshots appear within a .snapshot directory for NFS clients.
    -   **Authentication** - System (default)
    -   **Default Access (For All Clients)** - No Access
    -   Click on **Add Exceptions**
    -   **Clients with Read-Write Access** - The first three octets of your cluster network followed by an asterisk (ex., 10.38.62.\*)
    
    ![](/unified-storage/assets/3.28d1c5bb.png)
    
    By default, an NFS export will allow read and write access to any host that mounts the export, but this can be restricted to specific IPs or IP ranges, as we've done here.
    
4.  Review the **Summary** and click **Create**.
    

### [#](#testing-the-export) Testing The Export

You will use a Linux Tools VM as a client for your NFS Files export.

1.  Within Prism Central, select **\> Compute & Storage > VMs**. Identify the IP address assigned to the `USER##`\-LinuxTools VM using the _IP Addresses_ column.
    
2.  Open Putty, connect to your `User##`\-LinuxTools VM by entering the IP address within the _Host Name (or IP address)_ field, and click **Open**.
    
3.  Log in using the following credentials.
    
    -   **Username** - `root`
    -   **Password** - `nutanix/4u`
4.  Execute the following, which may be done using each command separately, or all at the same time. Any text after the `#` in each line is safe to leave in, as it won't be included as a part of the command.
    
    ```
    # Use Vault Mirror
    sudo sed -i s/mirror.centos.org/vault.centos.org/g /etc/yum.repos.d/*.repo
    sudo sed -i s/^#.*baseurl=http/baseurl=http/g /etc/yum.repos.d/*.repo
    sudo sed -i s/^mirrorlist=http/#mirrorlist=http/g /etc/yum.repos.d/*.repo
    
    sudo su - centos #Elevates our privileges
    sudo yum install -y nfs-utils #Installs the NFSv4 client
    mkdir /home/centos/filesmnt #Creates a new directory
    sudo mount.nfs4 BootcampFS.ntnxlab.local:/`User##`-logs /home/centos/filesmnt #Ensure you modify the User## to match yours. Mounts the NFS the directory we just created on our Linux machine
    df -kh #Lists the files & folders within this folder
    ll /home/centos/filesmnt/ #Shows the number of files within this folder
    ```
    
5.  Observe that the **logs** NFS share is mounted in `/home/centos/filesmnt`.
    

6.  The following command will add 100 2MB files filled with random data to `/filesmnt/logs`:
    
    ```
    sudo su - centos
    mkdir /home/centos/filesmnt/host1
    for i in {1..100}; do dd if=/dev/urandom bs=8k count=256 of=/home/centos/filesmnt/host1/file$i; done
    ```
    
7.  Return to **Nutanix Files** browser tab.
    
8.  Click on **Shares > `User##`\-logs** to monitor performance and usage of you NFS export. The utilization data is updated every ten minutes. We've provided an example should you not wish to wait for your data to display.
    
    ![](/unified-storage/assets/4.302a5168.png)
    
9.  Click the at the top left corner once you are ready to proceed to the next section.