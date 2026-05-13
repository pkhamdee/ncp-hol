# Upload Document to Object Store

1.  On your VDI workstation, download the sample document by right clicking and saving [this linkopen in new window](http://10.42.194.11/workshop_staging/tradeshows/experimental/nai-bootcamp/Release-Notes-Nutanix-Enterprise-AI-v2_5.pdf).
    
    !!! tip    
        If Chrome displays a warning, click on **Keep**
    
        ![Insecure Warning](images/insecure-download.b8e7a5e7.png)
    
2.  If not already open, open a new tab in your browser to the URL of the Objects browser, which you obtained from the previous section.
    
    ![Objects Browser](images/objects-browser.f7413680.png)
    
3.  Enter in the shared Access Key and Secret Key for the Nutanix Objects bucket that you obtained from the previous section and click **Login**.
    
4.  Click on the bucket that corresponds with your username.
    
5.  Click **Upload Objects** > **Select Files**.
    
    ![Objects Upload](images/objects-upload.03a6ef63.png)
    
6.  Select the PDF you downloaded in step 1.
    
7.  When it's done uploading, click **Close**.
    
    ![Completed Objects Upload](images/objects-upload-done.7c5edc16.png)
    

Now we're ready to configure the document store.