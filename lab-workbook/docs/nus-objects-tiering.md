# Nutanix Unified Storage

# [#](#objects-tiering) Objects: Tiering

## [#](#overview) Overview

Tiering helps keep costs down and supports a hybrid cloud strategy. It is possible to configure tiering in Nutanix Objects so that when objects reach a certain age, they are automatically and transparently moved to another location, or "endpoint." The endpoint can be in the public cloud (AWS, Azure, GCP) or another on-premises object store, so long as it's strictly S3 compliant.

## [#](#possible-tiering-configurations) Possible Tiering Configurations

Nutanix Objects is capable of tiering to any S3-compatible objects store provider.

## [#](#requirements) Requirements

You will need either an existing or new AWS account and the following to accomplish this tiering.

-   **Source S3 Storage**
    
    -   Source S3 access URL
    -   Source Access key
    -   Source Secret key
-   **Destination S3 Storage**
    
    -   Destination S3 access URL
    -   Destination Access key
    -   Destination Secret key
-   **Networking between source and destination**
    
    -   Physical connectivity
    -   Firewall and security allowing for this connection

The below diagram illustrates another use case for Objects tiering. A high-performance all-flash Nutanix Objects cluster is inserted between the application and a low-performance, high-capacity object store. Application performance is massively accelerated because the app is now performing I/O against high-performance storage. However, thanks to the backend tiering, the data ultimately ends up on the high-capacity object store, where it can be stored cost-effectively.

![](/unified-storage/assets/tieringdesign2.16f93840.png)

## [#](#lab-setup) Lab Setup

In this lab, you will set up tiering from a local Nutanix Objects bucket to a bucket in AWS S3.

![](/unified-storage/assets/tieringdesign3.4c1cedf6.png)

At a high level, we will implement the following:

-   Create an AWS bucket as a tiering destination
-   Setup Endpoint in Object Store configuration
-   Setup Lifecycle policies within the bucket configuration

### [#](#creating-aws-s3-bucket-as-a-tiering-destination) Creating AWS S3 Bucket As A Tiering Destination

In this section, you will configure the AWS S3 bucket, set up access permissions and get access and secret keys.

You can quickly set up an AWS account or use your existing account (if you have one) and perform the following tasks using the AWS Free Tier program. Just be careful to ensure the amount of data written remains under 50 MB

#### [#](#create-an-aws-s3-bucket) Create an AWS S3 Bucket

1.  Sign in to the AWS Management Console and open the Amazon S3 console at [https://console.aws.amazon.com/s3/open in new window](https://console.aws.amazon.com/s3/).
    
2.  Choose **Create bucket**.
    
3.  In the Bucket name, enter your initials (**lnb** here is an example) **lnb-bucket** DNS-compliant name for your bucket name.
    
    ![](/unified-storage/assets/tiering1.53c06b6f.png)
    
4.  Choose your preferred region (in most cases, this is automatically selected). Choose the closest region for tiering.
    
5.  Choose **Block all public access**.
    
6.  Choose to **Disable** Bucket versioning.
    
7.  Choose to **Disable** Server-side encryption.
    
8.  Click the **Create Bucket** button at the bottom of the page. You will now see your bucket in the list.
    
    ![](/unified-storage/assets/tiering2.e8df0ac3.png)
    

#### [#](#setup-access-for-an-aws-s3-bucket) Setup Access For An AWS S3 Bucket

1.  Go to your AWS IAM Management Console [hereopen in new window](https://console.aws.amazon.com/iamv2).
    
2.  Select **Users > Add User**.
    
3.  Enter **lnb-bucket-user** as the user name.
    
4.  Select **Access key - Programmatic access**.
    
5.  In the next window, select **Add user to group**.
    
6.  Since we don't have a group yet, click on **Create group**.
    
    ![](/unified-storage/assets/tiering3.faf915bb.png)
    
7.  Enter **s3access** as the user group name.
    
8.  In the **Filter Policies** input type **s3** and choose **AmazonS3FullAccess** as the policy which provides all permissions. Feel free to explore other permission policies as well.
    
    ![](/unified-storage/assets/tiering4.9773595f.png)
    
9.  Click on **Create group**.
    
10.  Choose the **s3access** as the user group name and click on **Next: Tags** at the bottom of the screen.
    
    ![](/unified-storage/assets/tiering5.18e70ed8.png)
    
11.  Click on **Next: Review**.
    
12.  Click on **Create user**.
    
13.  You will now see a success message followed by download options for the access and secret key.
    
14.  Download access and secret key CSV file.
    
    Note
    
    Make sure to download this CSV file and store it securely, as it will be only possible to do this once
    
    ![](/unified-storage/assets/tiering6.54c4a74c.png)
    
15.  Click on **Close**
    

You have successfully set up access to your AWS S3 bucket.

### [#](#setup-endpoint-in-object-store-configuration) Setup Endpoint In Object Store Configuration

#### [#](#configure-endpoint) Configure Endpoint

1.  Login into your Prism Central instance.
    
2.  Navigate to **\> Services > Objects**.
    
3.  Choose your Objects store.
    
    ![](/unified-storage/assets/tiering7.6efa0f87.png)
    
4.  This will open a new browser tab with additional settings for your chosen objects store.
    
5.  Select **Tiering Endpoint** and click on **Create**
    
    If you are the first person to create a tiering endpoint, click on **+Add**
    
    ![](/unified-storage/assets/tiering8.e0b52641.png)
    
6.  In the add endpoint wizard, enter the following details.
    
    -   Name of the Endpoint - **AWS Tiering Endpoint** (give an easily identifiable name)
    -   Endpoint Type - **Nutanix Objects**
    -   Service Host - **s3.ap-southeast-2.amazonaws.com** (this will change depending on your AWS region)
    -   Bucket Name - **lnb-bucket** (this is the name of the bucket you created in the previous section in AWS)
    -   Access Key - **access key from CSV you downloaded** in the previous section
    -   Secret Key - **secret key from CSV you downloaded** in the previous section
    -   Skip SSL certificate validation - **Checked**
    
    ![](/unified-storage/assets/tiering9.1c282ae1.png)
    
7.  Click on **Save**.
    
8.  You will now be able to see the endpoint in your Object Store configuration.
    
    ![](/unified-storage/assets/tiering10.ebc76fe5.png)
    

You have successfully set up a tiering endpoint that resides in AWS.

#### [#](#configure-lifecycle-policies) Configure Lifecycle Policies

Lifecycle policies allow tiering to be scheduled from the source bucket to the target bucket, irrespective of the location.

In this section, we will create a lifecycle policy to tier data from Nutanix Object's bucket that you created in [Objects Versioning Access Control](/unified-storage/objects_versioning_access_control/objects_versioning_access_control.html) to the AWS bucket you created earlier.

1.  Within Prism Central, navigate to **\>Services > Objects**.
    
2.  Choose your Objects store.
    
3.  Click your source bucket _your-name_\-**my-bucket** (the one you created in here [here](/unified-storage/objects_buckets_users_access_control/objects_buckets_users_access_control.html).
    
    ![](/unified-storage/assets/tiering11.db354b34.png)
    
4.  Click on **Lifecycle** and click on **Create Rule**.
    
    ![](/unified-storage/assets/tiering12.f6b1be64.png)
    
5.  Enter a meaningful name that you can identify, for example, **tier-to-aws-ap-southeast-2.amazonaws.com**, which specifies the region of tiered data.
    
6.  Choose **All Objects**.
    
    Note
    
    You can also use tags as an option to select the objects to tier.
    
    ![](/unified-storage/assets/tiering13.c532d1fd.png)
    
7.  Click on **Next**.
    
8.  Select **AWS Tiering Endpoint**.
    
9.  Set tiering to **1** days after the objects creation date in the source bucket.
    
10.  You can also select expiration to **2** days in the destination storage, as an example, to make sure you don't run into a massive bill in the public cloud for testing purposes.
    
11.  Click on **Add Action** and choose another expire Action.
    
12.  Choose **Multipart Uploads** and **2** days after last creation date on destination bucket.
    
    ![](/unified-storage/assets/tiering14.790a34da.png)
    
13.  Click on **Next**.
    
14.  Review your configuration and click on **Done**.
    
    ![](/unified-storage/assets/tiering15.b987ae4e.png)
    

#### [#](#verify-tiering) Verify Tiering

This section will verify the tiering status on the source and destination sides.

1.  Since your source bucket is already populated with data, the tiering will start after one day.
    
    Note
    
    If you are only doing the Objects Tiering lab:
    
    -   Create your source bucket using the procedure in _Create Bucket In Prism_ section in [this section](/unified-storage/objects_versioning_access_control/objects_versioning_access_control.html)
    -   Populate your source bucket with objects (data) using procedure _Uploading Multiple Files to Buckets with Python)_ in [Objects CLI Scripts](/unified-storage/objects_cli_scripts/objects_cli_scripts.html)
    
2.  Once tiering is successful, you will see Tiering status on your source bucket **your-name-my-bucket > Summary**.
    
    ![](/unified-storage/assets/tiering16.a4f06c7a.png)
    
3.  On the destination AWS **lnb-bucket**, you will see data as follows: note that this may differ for your bucket.
    
    ![](/unified-storage/assets/tiering17.c20a9e52.png)
    

You have successfully tiered from Nutanix Objects to AWS S3.

## [#](#takeaways) Takeaways

What are the key things you should know about **Nutanix Objects Tiering(Lifecycle Policies)**?

-   Nutanix Objects allows easy configuration for tiering data to other object stores (cloud and on-premises)
-   Tiering policies need to be configured at the source (provider) of the bucket
    -   for example: Tiering from Nutanix Objects to AWS needs to be configured at Nutanix PC
    -   for example: Tiering from AWS S3 to AWS Glacier needs to be configured at AWS Console
-   Nutanix enables applications to store and tier data to any S3-based object storage without lock-in
-   Nutanix Object tiering feature groups objects together in a larger data chunk to save costs on PUT requests in S3