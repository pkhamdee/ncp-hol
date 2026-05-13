# Configure Embeddings and Vector Store

!!! tip

    The inputs you'll need can be found on the Connection Details page or from your instructor. This time, we'll be using the **Embedding** model.

    -   BaseURL: `Shared NAI Endpoint URL`
    -   Model Name: `Shared NAI Embedding Model Name`
    -   OpenAI API key: `Shared NAI API Key`

## View Chunks

1.  Your Document Loader should currently show 17 chunks. To view the chunks, click on the name of the Document Loader.
    
    ![Document Chunks](images/document-chunks.afdb2f84.png)
    
2.  Click the purple **Back** arrow.
    

## Configure Embeddings

1.  Under the Actions column in the **documents** table, click **Options** > **Upsert Chunks**
    
    ![Upsert Chunks](images/upsert-chunks.f8dc7631.png)
    
2.  Click on **Select Embeddings**
    
    ![Select Embedding](images/configure-embeddings.20eae8e4.png)
    
3.  Search for **OpenAI Embeddings Custom**
    
    ![OpenAI Embeddings Custom](images/openai-embeddings-custom.764f9f19.png)
    
4.  Under **Connect Credential**, select the Shared Key credential you configured in the [Configure Shared Endpoint](/nai/application/create-chatbot/use-gpu.html) section.
    
5.  Under **BasePath**, enter the `Shared NAI Endpoint URL`
    
6.  Under **Model Name**, enter in the `Shared NAI Embedding Model Name`.
    

Your screen should look similar to the below.

![Configured Embeddings](images/after-embeddings.11db74f0.png)

!!! warning

    Ensure you are using the shared **Embedding** model as referenced above. Otherwise, you will encounter a 504 error when upserting.

## [#](#configure-vector-store) Configure Vector Store

1.  Click on **Select Vector Store**.
    
2.  Select **Milvus**.
    
3.  Under Connect Credential, click **Create New**
    
4.  Enter in root for the credential name and Milvus user and leave the password blank, then click **Add**.
    
    ![Milvus Auth](images/milvus-auth.564c6d09.png)
    
    !!! tip
    
        We are using the root user for demonstration purposes. In production, it is not recommended to use the root user.
    
5.  Under **Milvus Server URL**, enter in the URL to the Milvus Database corresponding with your user.
    
    !!! warning
    
        Do not use the URL with **attu** in the name, that is for the UI and not the database.
        
        Do not include a trailing slash at the end of the URL.
    
6.  Under **Milvus Collection Name**, enter in **docsuser`##`** where `##` corresponds to your user number.
    
7.  In the top right, click **Save Config**, and then click **Upsert**.
    
    ![Save and Upsert](images/save-and-upsert.948d89c5.png)
    
8.  On a successful upsert, you should see a screen like the following. Click **Close**.
    
    ![Successful Upsert](images/successful-upsert.12cb21bf.png)
    

Now you are ready to connect your chatflow to the vector database.

## View Documents in Milvus (optional)

1.  Open a new tab in your browser and navigate to the Milvus Attu UI that corresponds to your database.
    
2.  Click **Connect** to login.
    
    ![Milvus Login](images/milvus-login.d7400e67.png)
    
3.  Click on the **default** database.
    
4.  You should see your collection that was created by the upsert process.
    
5.  Click on the **unloaded** label, and then click **Load**.
    
    ![Load Collection](images/milvus-unloaded.50efd291.png)
    
6.  Once the status changes to **loaded**, click on the collection name, and then click the **Data** tab.
    
7.  Scroll to the right to view the text and the corresponding vectors.
    
    ![Vectors](images/milvus-vectors.ac30bc1e.png)