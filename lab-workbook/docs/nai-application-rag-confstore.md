# Configure Document Store

## Create document store

1.  From your chatflow, make sure the chatflow is saved, and then click the back arrow.
    
    ![Chatflow Back Button](images/chatflow-back-button.7d402437.png)
    
2.  Click Document Stores in the menu on the left. If the menu is hidden, click the purple hamburger icon next to the FlowiseAI logo.
    
    ![Navigate to Document Stores](images/navigate-to-ds.8b3ede50.png)
    
3.  Click **+Add New** in the top right.
    
    ![Add New Document Store](images/add-new-ds.ce91ec5b.png)
    
4.  Give the Document Store a name, e.g. `documents`, and then click **Add**.
    
    ![Add Document Store](images/add-new-ds-dialog.676c0f8a.png)
    

## Add a document loader

1.  Click into the Document Store and click **\+ Add Document Loader**
    
    ![Add Document Loader](images/add-document-loader.642da441.png)
    
2.  Search for **S3** and click on **S3 Directory**. This will let us configure the Object Storage bucket to use.
    
    ![S3 Directory](images/s3-directory.86d4a89e.png)
    

## Configure document loader

1.  Click the drop-down for **Credential** and click **Create New**.
    
2.  Enter in a name for your credential (e.g. `objects-creds`)
    
3.  Under AWS Access Key and AWS Secret Key, enter in your Access and Secret Key for your Nutanix Objects bucket that you obtained in the [Obtain Object Store URL and Credentials](nai-application-rag-store.md) section and click **Add**.
    
    ![S3 Credentials](images/s3creds.8d6c1c03.png)
    
4.  Under **Bucket**, enter in your bucket name, e.g. `adminuser##`
    
5.  Leave the region at its default.
    
6.  Under **Server URL**, enter in your object store IP, e.g. `http:/#.#.#.8`
    
    !!! warning    
        In the lab we are using self-signed certificates, so ensure you are using `http` and not `https`.
    
7.  Under **Select Text Splitter**, select **Recursive Character Text Splitter** from the drop-down.
    
    ![Text Splitter](images/doc-store-text-splitter.604f8d7c.gif)
    
    At this point, your screen should look similar to this:
    
    ![S3 Directory Config](images/s3-configuration.10dd1f7b.png)
    
8.  On the right hand side, click **Preview Chunks**.
    
9.  The preview chunks should show up on the right hand side. If not, double check the inputs for your Objects connection.
    
    ![Preview Chunks](images/s3-preview-chunks2.343742c4.png)
    
10.  In the top right corner, click **Process**.
    
    ![Process Button](images/s3-process-button.c3fd4fcd.png)
    
11.  The document store will show as "stale" with a yellow circle icon. Click on the blue refresh button.
    
    ![Refresh](images/s3-doc-store-refresh.70f6c46c.png)
    
12.  The "stale" status should change to "sync" and the icon should now be green. If not, please double check the Document Loader settings.
    
    ![Successful Sync](images/s3-synced.a7258f62.png)