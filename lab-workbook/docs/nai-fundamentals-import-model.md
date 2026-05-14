# Import Your First Model

## View Available Models

1.  From the Nutanix Enterprise AI console, click on **Models**.
    
2.  Click on **Import from NVIDIA NGC Catalog.**
    
    !!! info
    
        Model Import Options
        
        You can import models directly from Hugging Face or the NVIDIA NGC Catalog. Alternatively, you can also upload custom model files or pre-validated models from a local NFS, SMB, or S3 store.
    
3.  View the list and types of available models. Click on **Filter by Capabilities** to see the types of models. We'll be using both **Text to Text** and **Embedding** models later in our RAG application.
    
    ![Model Capabilities](images/model-capabilities.dd197ee1.png)
    
    !!! tip    

        The model types are defined in the [appendix](nai-appendix-typellm.md).
    
4.  Admin users can control which models are accessible to users. Hover over one of the radio buttons, and notice that the model import has been restricted by the admin.
    
    ![Restricted Model](images/model-restricted.922a5249.png)
    
5.  Click **Cancel**.


## Import Hugging Face Model

1.  Click **Import from Hugging Face Model Hub**.
    
2.  Click the radio button next to `meta-llama/Llama-3.2-1B-Instruct`
    
    !!! note    
        This model is selectable because the admin has enabled access to this model. Note that you cannot select any other model.
    
3.  Click **Import**.
    

![Import Screen](images/import-screen.43e04582.png)

1.  Name the model `llama32-1b##`, where `##` corresponds to your username, and click **Import**.

![Import Dialog](images/import-dialog.f1d6367f.png)

Since the model is small it will download relatively quickly. Larger models take longer to download.

!!! info
    The stages of downloading a model

    -   **Pending** - validates tokens and sets up storage by creating a PVC and provisioning a share on the file server (in this case, Nutanix Files)
    -   **Processing** - connects to HuggingFace and downloads the model files
    -   **Ready** - model is ready to use

Once the model is marked as **Ready**, it is ready to use. Wait until the model is marked as ready before moving to the next step.

![Model Active](images/ready-model.241bb89e.png)

!!! info
    How does NAI connect to Nutanix Files?

    To enable persistent storage, the Nutanix Enterprise AI instance uses a storage class configured with the Nutanix CSI driver to connect to Nutanix Files.