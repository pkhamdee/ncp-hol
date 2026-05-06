# Nutanix Unified Storage

# [#](#objects-versioning-and-access-controls) Objects: Versioning And Access Controls

## [#](#accessing-and-creating-buckets-with-objects-browser) Accessing And Creating Buckets With Objects Browser

In this exercise, you will use Objects Browser to create and use buckets in the object store using your generated access key.

### [#](#download-the-sample-images) Download the Sample Images

1.  Copy this link `http://10.42.194.11/hol/unified-storage/SampleData_Small.zip` and paste it within a new Chrome browser tab. This will download sample files to your Win-Tools VM.
    
2.  Open the Windows File Explorer. Within the _Downloads_ folder, right-click on **SampleData\_Small.zip**, and choose **Extract All**.
    
3.  Click **Browse**, select your **Downloads** folder, and click **Extract**.
    

### [#](#use-object-browser-to-create-a-bucket) Use Object Browser To Create A Bucket

1.  Return to your Objects Store browser tab. Click on **Launch Objects Browser**. The Objects Browser will open in a new tab.
    
2.  Enter the **Access Key** and **Secret Key** information and click **Login**.
    
3.  Click **Create Bucket**.
    
4.  Enter the following name for your bucket, and click **Create**.
    
    -   **Bucket Name** - `user##`\-test-bucket
    -   **Enable versioning** - checked
    
    ![](/unified-storage/assets/1.e9098eda.png)
    
    Creating a bucket in this fashion allows for self-service for entitled users. It is no different from a bucket created via the Prism Buckets UI.
    
5.  Click **`user##`\-test-bucket**.
    
6.  Choose **Select Folder** from the _Upload Objects_ drop-down.
    
    ![](/unified-storage/assets/2.81578a16.png)
    
7.  Navigate to your Downloads directory, and within the _Sample Data_ folder, select the **Pictures** folder. Once the uploads have been completed, click **Close**.
    

## [#](#object-versioning) Object Versioning

Object versioning allows the upload of new versions of the same object for required changes without losing the original data. Versioning can be used to preserve, retrieve and restore every version of every object stored within a bucket, allowing for easy recovery from unintended user actions and application failures.

1.  Within your Remote Desktop session, open Notepad.
    
2.  Type `version 1.0`. Save the file with **version.txt** as the name within your _Downloads_ folder.
    
3.  Return to the Objects Browser tab. Upload the file. Once the uploads have been completed, click **Close**.
    
4.  Return to Notepad. Replace the text with `version 2.0`, and save it using the same file name and location.
    
5.  Within your Objects Browser tab, upload the modified file.
    
6.  Select your **version.txt** file. Within the _Actions_ drop-down, select **Versions**.
    
7.  Notice that both versions of the file were preserved, along with a _Latest_ tag to identify the latest version.
    
8.  Click **Close**, and **Back to Object Store**.