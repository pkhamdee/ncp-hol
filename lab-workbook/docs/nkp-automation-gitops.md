# NKP Advanced Hands-on Lab

Note

-   Expected lab duration: 6 minutes

# [#](#set-up-gitops-deployment) Set up GitOps Deployment Lab

Until now, all the steps you've performed have been by executing commands using _kubectl_, or on the UI doing clicks. These manual deployments bring the following challenges:

1.  **Inconsistencies**: Changes made directly to the cluster can drift from documentation or intended configurations.
    
2.  **Lack of Automation**: Manual updates require repetitive tasks prone to human error.
    
3.  **Limited Traceability**: There’s no inherent record of who changed what and why.
    

---

When implementing GitOps, the application manifests are stored in a Git repository. Using a Continuous Delivery or Deployment (CD) tool, the manifests can be automatically deployed and synced to a Kubernetes cluster.

We will use NKP's built-in GitOps tool, FluxCD to sync the repository with the Kubernetes cluster. This allows us to manage updates by making changes to the repository and allowing the GitOps tool to apply them automatically.

Did You Know?

**GitOps** requires NKP Ultimate

#### [#](#create-a-gitops-source) Create a GitOps Source

1.  From the left-hand menu in the NKP console, select Projects and open the project named `user##` corresponding to your username. (**user16** shown in the screenshot below)
    
    ![Projects overview](/cloudnative/assets/projects_overview.95db64a9.png)
    
    Note
    
    For a refresher on Projects, please visit this [Chapter](/cloudnative/automation/fundamentals/multitenancy-basics/nkp-projects.html).
    
2.  Navigate to **Continuous Deployment (CD)** tab and click `Add GitOps Source`.
    
    ![Add GitOps source](/cloudnative/assets/add_source.9521f23c.png)
    
3.  On the GitOps Source configuration page, provide the following details:
    
    -   **ID (name)**:
    
    ```
    online-boutique
    ```
    
    -   **Repository URL:**
    
    ```
    https://github.com/nutanixdev/nkp-microservices-demo.git
    ```
    
    -   **Git Ref Type:** `Branch`
        
    -   **Branch Name:**
        
    
    ```
    main
    ```
    
    -   **Path:**
    
    ```
    ./release/without-istio
    ```
    
    Ensure the path is exactly as seen above.
    
4.  Once the details are filled in, save the configuration. This will set up the GitOps Source for your project.
    
    ![GitOps source](/cloudnative/assets/boutique_gitops_source.609165ea.gif)
    

The application that you will be deploying is an online boutique app, which is a microservices-based reference application designed to illustrate the complexities and best practices of managing and deploying distributed systems. The app is composed of 11 different microservices, each written in different programming languages and frameworks.

Did you know?

Platform Engineers can enable developer self-service and automation while providing a consistent and flexible developer experience.

-   **Staging Workflow**: Code pushed to the staging directory is synced by FluxCD to staging clusters tied to the staging Project.
-   **Production Workflow**: Code pushed to the production directory is synced by FluxCD to production clusters tied to the production Project.
-   **Namespace as a Service**: Adding a new cluster to a Project automatically applies the application and namespace configurations to it.

![GitOps Sync](/cloudnative/assets/gitops_sync.fe9e53be.png)