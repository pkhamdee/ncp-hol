# Nutanix Unified Storage

# [#](#objects-buckets-users-and-access-control) Objects: Buckets, Users, And Access Control

## [#](#create-bucket-in-prism) Create Bucket In Prism

A bucket resides within an object store which can have policies applied to it, such as versioning and WORM. A newly created bucket is a private resource to the creator by default. By default, the bucket's creator has read/write permissions. Access permission can also be granted to other users.

1.  Within _Prism Central_, navigate to **\> Services > Objects**.
    
2.  Click on the **ntnx-objects** Objects Store. The Objects Store management will open in a new browser tab.
    
3.  Click **Create Bucket**. Fill out the following fields, and click **Create**.
    
    -   **Name** - `user##`\-bucket (lower case)
    -   **Enable Versioning** - Checked
    
    ![](/unified-storage/assets/1.8ed30024.png)
    
    Bucket names must be lowercase and only contain letters, numbers, periods, and hyphens.
    
    Additionally, all bucket names must be unique within a given Object Store.
    
    If versioning is enabled, new versions of the same object can be uploaded for required changes without losing the original data. Lifecycle policies define how long to keep data in the system.
    
    Once the bucket is created, it can be configured with WORM (Write Once, Read Many). WORM prevents editing, overwriting, renaming, and deleting data, which is crucial in heavily regulated industries (finance, healthcare, public agencies, etc.) where sensitive data is collected and stored. Examples include e-mails, account information, voice mails, and more.
    
    Note
    
    If WORM is enabled on a bucket, it will supersede any lifecycle policy.
    
4.  Check the box next to **`user##`\-bucket**. Within the _Actions_ drop-down, choose **Configure WORM**
    
    ![](/unified-storage/assets/2.bcfd3c20.png)
    
5.  Check the box next to **Enable WORM**.
    
6.  Within the _Retention Period_ field, enter **1**. Select **months** from the adjacent drop-down. Click **Enable WORM**.
    
    Note
    
    You can define a WORM data retention period on a per-bucket basis.
    
    ![](/unified-storage/assets/2a.0ae277b7.png)
    

## [#](#objects-user-management) Objects User Management

In this exercise, you will generate access and secret keys to access the object store used throughout the lab.

1.  Return to your _Prism Central_ browser tab.
    
2.  Click on **Access Keys** from the top menu, and click **Add People**.
    
    ![](/unified-storage/assets/3.98c7c525.png)
    
3.  Select **Add people not in a directory service**. Enter your e-mail address and name. Click **Next > Generate Keys**.
    
    ![](/unified-storage/assets/4.061d21c2.png)
    
4.  Click **Download Keys** to download a .txt file containing the **Access Key** and **Secret Key**. Click **Close**.
    
5.  Open the file by clicking on the file name in the lower left corner of Chrome.
    
    ![](/unified-storage/assets/5.f814312b.png)
    

## [#](#adding-users-to-buckets-share) Adding Users To Buckets Share

1.  Return to your Objects Store browser tab.
    
2.  Click **`user##`\-bucket**, and select **User Access** from the left menu.
    
3.  Click **Edit User Access** to share your bucket. You can configure read access (GET), write access (PUT), or both on a per-user basis.
    
4.  Add your e-mail address. Select it, and within the _Permissions_ drop-down, select both **Read** and **Write** permissions. Click **Save**.
    
    ![](/unified-storage/assets/6.feaa5714.png)
    
5.  Click **Back** in the top left corner.