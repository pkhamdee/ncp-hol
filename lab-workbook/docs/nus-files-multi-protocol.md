# Nutanix Unified Storage

# [#](#files-multi-protocol) Files: Multi-Protocol

A Nutanix Files share has the concept of a native and non-native protocol. All permissions are applied using the native protocol. Any access requests using the non-native protocol require a user or group mapping to the permission applied from the native side. There are several ways to apply user and group mappings, including rule-based, explicit, and default ones. You will first configure a default mapping.

1.  Navigate to **Configuration > Authentication**
    
2.  Select the **User Mapping** tab.
    
3.  Within the _Default Mapping_ section, click on **Edit**
    
    ![](/unified-storage/assets/1.952502e6.png)
    
4.  Select **Deny access to NFS export** and **Deny access to SMB share** as the defaults for when no mapping is found. Click **Save**.
    
    ![](/unified-storage/assets/2.ca9ce8e1.png)
    
5.  Click on **Shares > `User##`\-Marketing**.
    
6.  Click **Update**.
    
7.  Check **Enable multiprotocol access for NFS**, and click **Next**.
    
    ![](/unified-storage/assets/3.9449c398.png)
    
8.  Within the _Multi Protocol Access_ section, check **Allow simultaneous read access to the same files**, and click **Next**.
    
    ![](/unified-storage/assets/4.05d7fff3.png)
    
9.  Click **Update**.
    
    The multiprotocol configuration for your `User##`\-Marketing share is complete. Now we will try accessing the share to test multiprotocol configuration.
    
10.  Return to your `User##`\-LinuxTools VM Putty session.
    
11.  Execute the following. Be sure to modify `User##` with your own.
    
    ```
    sudo mkdir /filesmulti
    sudo mount.nfs4 BootcampFS.ntnxlab.local:/`User##`-Marketing /filesmulti
    dir /filesmulti
    ```
    
    You will now add an explicit mapping to allow access to the non-native NFS protocol user. We must get the user ID (UID) to create the explicit mapping. Because the default mapping is to deny access, so the **Permission denied** error is expected.
    
12.  Execute the `id` command, and take note of the UID.
    
13.  Return to your **Nutanix Files** browser tab, click the at the top left corner.
    
14.  Click on **Configuration > Authentication**, and select the **User Mapping** tab.
    
15.  Within the _Explicit Mapping_ section, click on **Configure**.
    
16.  Click on **Add one-to-one mapping**.
    
17.  Fill out the following fields, click , and click **Save**.
    
    -   **SMB Name** - `ntnxlab\administrator`
    -   **NFS ID** - UID from step 12 (ex. 1000)
    -   **User/Group** - User
    
    ![](/unified-storage/assets/5.1ab601f3.png)
    
18.  Return to your `User##`\-LinuxTools VM Putty session. Execute the `dir /filesmulti` command, which will now complete.
    
    ![](/unified-storage/assets/6.b7c87879.png)
    

You have successfully configured multiprotocol access for your `User##`\-Marketing share.