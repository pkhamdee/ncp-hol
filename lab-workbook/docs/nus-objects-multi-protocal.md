# Nutanix Unified Storage

# [#](#objects-multiprotocol) Objects: Multiprotocol

This section will show how to create a multiprotocol (NFS 3.0, S3) bucket and configure a Network File System (NFS) client to access it to perform folder and file operations. The prominent use cases for this feature are:

-   Consolidate old backups (written to NFS export) and new backups (written to S3 bucket).
-   Facilitate in-place analytics of data that was written over a protocol not supported by the analytics application. For example, log data written to NFS by a legacy app can instantly be accessed by a big data analytics engine that requires access via the S3 API.

## [#](#configure-nfs-allow-client) Configure NFS Allow Client

In this section, we will allow the **`User##`\-LinuxTools** VM to access buckets using the NFS3 protocol.

1.  Return to the **Objects Store** management tab.
    
2.  Click on **NFS Clients Allowlist** from the left menu.
    
3.  Click on **Add Client**.
    
    ![](/unified-storage/assets/1.14d5dad3.png)
    
4.  Enter your `User##`**\-LinuxTools** IP address followed by `/32` to specify access only to this client (ex., 10.38.62.76/32)
    
    ![](/unified-storage/assets/2.6b3e25bc.png)
    
5.  Click on **Add > Save**.
    

## [#](#create-a-bucket) Create A Bucket

A bucket is a sub-repository within an object store that can have policies applied to it, such as versioning or write once read many (WORM). A newly created bucket is a private resource to the creator by default. By default, the creator of the bucket has read/write permissions and can grant permissions to other users.

1.  Click on **Buckets** from the left menu.
    
2.  Click **Create Bucket**. Fill out the following fields.
    
    -   **Name** - **`user##`\-multi**
    -   **Enable NFS v3 access** - checked
    -   **Owner UID** - `8888`
    -   **Owner GID** - `8888`
    
    Click the **Default Access Permissions** drop-down, and select the following.
    
    -   **Files**
        -   **Owner** - read, write, execute
        -   **Group** - read, write, execute
        -   **Others** - read, write, execute
    -   **Directory**
        -   **Owner** - read, write, execute
        -   **Group** - read, write, execute
        -   **Others** - read, write, execute
    
    Click **Create**.
    
    ![](/unified-storage/assets/3.c33d5640.png)
    

## [#](#adding-users-to-buckets-share) Adding Users To Buckets Share

In this section, we will add a user to the `user##`\-multi bucket, so we can access the bucket to upload/create files and folders.

1.  Select `user##`\-multi, and within the _Actions_ drop-down, choose **Share**.
    
2.  Enter your e-mail address, select _Read_ and _Write_ permissions, and click **Save**.
    
    ![](/unified-storage/assets/4.ff213651.png)
    

## [#](#accessing-bucket-on-nfs-client) Accessing Bucket On NFS Client

This section will mount the `user##`\-multi bucket as an NFSv3 share on the **`User##`\-LinuxTools** VM to create files and folders.

1.  Return to your **`User##`\-LinuxTools** SSH session.
    
2.  Execute the `yum install -y nfs-utils` command to enable NFS utilities.
    

3.  Execute the `mkdir -p /mnt/buckets` command to create a directory on our LinuxTools VM.
    
4.  Execute the `nano /etc/fstab` command. Add this line to the end of the file.
    
    ```
    <OBJECT-STORE-IP>:/user##-multi /mnt/buckets nfs rw  0 0
    ```
    
5.  Press **CTRL + X**. Press **y** followed by **Enter** to save the file.
    
6.  Execute the `mount -a` command to mount the **`user##`\-multi** bucket to the local **/mnt/buckets** folder as an NFS share.
    
7.  Execute the following to create a directory and generate files.
    
    ```
    cd /mnt/buckets && mkdir mydir1 && cd mydir1
    for i in {1..10}; do echo "writing file$i .."; touch file$i.txt; echo "this is file$i" > file$i.txt; done
    ```
    
8.  Return to the Objects Browser tab.
    
9.  Click **`user##`\-multi**. Observe that the **mydir1** folder is present.
    
10.  Click on **mydir1**. Observe that the files are present.
    
    ![](/unified-storage/assets/5.7be748e5.png)
    
11.  Click **New Folder**, with the name `mysubdir1`.
    
12.  Click on **Create**.
    
13.  Return to your SSH session. Execute the `ll` command. Observe that your **mysubdir1** is present.
    

You have completed this lab and tested multi-protocol access to a bucket.

## [#](#takeaways) Takeaways

-   NFS access is easy to set up and extremely useful in specific scenarios
-   NFS access should only be enabled in scenarios where the NFS client will not subsequently update the written files. In other words, it is suitable for use cases where the file data is instantly "write-cold."
-   NFS enablement is incompatible with other bucket features such as WORM and lifecycle policies.