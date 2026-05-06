# Nutanix Unified Storage

# [#](#files-create-smb-share) Files: Create SMB Share

## [#](#overview) Overview

In this exercise, you will create and test an SMB share used to support home directories, user profiles, and other unstructured file data, such as departmental shares commonly accessed by Windows clients.

## [#](#using-smb-shares) Using SMB Shares

### [#](#creating-the-share) Creating The Share

1.  Open Chrome, enter your Prism Central IP address, and log into **Prism Central** using the provided credentials.
    
2.  Navigate to **Services > Files**, and click directly on **BootcampFS**.
    
3.  Select **Shares & Exports** and then **New Share or Export**.
    
    ![](/unified-storage/assets/1.1a90ed6b.png)
    
4.  Fill out the following fields, and click **Next**.
    
    -   **Name** - `User##`\-Marketing (ex. User01-Marketing)
    -   **Description (Optional)** - Departmental share for marketing team
    -   **Share Path (Optional)** - This field allows you to specify an existing path to create the nested share. Leave blank.
    -   **Max Size (Optional)** - This field allows you to set a hard quota for the individual share. Leave blank.
    -   **Primary Protocol Access** - SMB
    
    ![](/unified-storage/assets/2.dc793f21.png)
    
5.  Select the following options, and click **Next**.
    
    -   **Enable Self Service Restore**
    -   **Enable Compression**
    
    ![](/unified-storage/assets/3.28d1c5bb.png)
    
    Enable Access Based Enumeration and click **Next**.
    
    ![](/unified-storage/assets/3a.0d1d5393.png)
    
    **Standard** shares are most appropriate for our example, as you create a single departmental share, meaning that all top-level directories and files within the share and connections to the share are served from a single File Server VM (FSVM).
    
    **Distributed** shares are appropriate for home directories, user profiles, and application folders. This type of share distributes top-level directories across all File Server VMs (FSVM) and load balances connections across all FSVMs within the Files cluster.
    
    **Access Based Enumeration (ABE)** ensures that only files and folders a given user has read access to are visible to that user, commonly enabled for Windows file shares.
    
    **Self Service Restore** allows users to leverage Windows Previous Version to quickly restore individual files to previous revisions based on Nutanix snapshots.
    
6.  Review the **Summary** and click **Create**.
    
    ![](/unified-storage/assets/4.4ea31b24.png)
    
7.  Leave this tab open, as we will be returning to it.
    

### [#](#testing-the-share) Testing The Share

1.  Return to your `User##`\-WinTools VM Remote Desktop session and log in with the following credentials:
    
    -   Username **administrator@ntnxlab.local**
    -   Password: **cluster password**
2.  Open the Windows File Explorer. Right-click the **SampleData\_Small.zip** within the _Downloads_ folder, and choose **Extract All**.
    
    ![](/unified-storage/assets/6.3c67cd16.png)
    
    -   The `NTNXLAB\Administrator` user was specified as a Files Administrator during the deployment of the Files cluster, giving it read/write access to all shares by default.
    -   Managing access for other users is no different from any other SMB share.
3.  Enter `\\BootcampFS.ntnxlab.local\User##-Marketing` (ex. \\\\BootcampFS.ntnxlab.local\\User01-Marketing) within the _Files will be extracted to this folder_ field, and click **Extract**. A new Windows File Explorer window will open.
    
    ![](/unified-storage/assets/7.58b105b7.png)
    
4.  Right-click **`User##`Marketing > Properties**.
    
5.  Select the **Security** tab and click **Advanced**.
    
    ![](/unified-storage/assets/8.87acbf50.png)
    
6.  Select **Users (BootcampFS\\Users)** and click **Remove**.
    
    ![](/unified-storage/assets/8a.007e813b.png)
    
7.  Click **Add**.
    
8.  Click **Select a principal** and specify **Everyone** in the _Enter the object name to select_ field. Click **OK**.
    
    ![](/unified-storage/assets/9.09dff69d.png)
    
9.  Fill out the following fields and click **OK**:
    
    -   **Type** - Allow
    -   **Applies to** - This folder only
    -   Select **Read & execute**
    -   Select **List folder contents**
    -   Select **Read**
    -   Select **Write**
    
    ![](/unified-storage/assets/10.dfcc1bc3.png)
    
10.  Click **OK > OK > OK** to save the permission changes.
    
    All users can now create folders and files within the Marketing share.
    
    It is typical for shares utilized by many people to leverage quotas to ensure fair use of resources. Files offers the ability to set either soft or hard quotas on a per share basis for individual users within Active Directory or specific Active Directory Security Groups.
    

### [#](#adding-share-level-quota) Adding Share Level Quota

1.  Return to the **Nutanix Files** browser tab.
    
2.  Click on the `User##`**\-Marketing** share to open the share details.
    
    ![](/unified-storage/assets/11.10d6479a.png)
    
3.  Within the _Actions_ menu, select **Add Quota Policy**.
    
    ![](/unified-storage/assets/13.de2438e0.png)
    
4.  Fill out the following fields and click **Add**.
    
    -   Select **User Group**
    -   **User or Group** - SSP Developers
    -   **Quota** - 10 GiB
    -   **Enforcement Type** - Hard Limit
    
    ![](/unified-storage/assets/14.054f5570.png)
    
    These steps will enforce quota limits on the shares for the Microsoft Active Directory (AD) user group **SSP Developers**. If anyone within this group exceeds 10 GB of storage, they cannot add any more storage. A **Soft** limit would issue a warning but not limit the user's behavior in any way.
    
5.  Review the **Capacity Summary**, **Performance Summary**, **Share Properties**, and **Features** sections to understand the available on a per share basis, including the number of files & connections, storage utilization over time, latency, throughput, and IOPS.
    
    ![](/unified-storage/assets/15.6a15bba2.png)
    
6.  Click the in the top left corner once you are ready to proceed to the next section.