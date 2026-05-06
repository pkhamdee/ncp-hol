# NKP Advanced Hands-on Lab

# [#](#storageclass-with-nutanix-csi) StorageClass with Nutanix CSI

A StorageClass is a **cluster-scoped** resource that provides a way to dynamically provision persistent storage (`PersistentVolumeClaim`) for applications running in the cluster. It acts as an abstraction layer between Kubernetes and the underlying storage infrastructure using a `CSI provisioner`, allowing users to define different types of storage with varying properties, such as performance, replication, or backup policies.

#### [#](#nkp-default-storageclass-configuration) NKP default StorageClass configuration

Let's check the StorageClass configuration for cluster **workload01**, because we'll use our project for deploying the application.

This time for interacting with the Kubernetes cluster and its resources, we'll use the Kubernetes Dashboard UI.

Did You Know?

**Kubernetes Dashboard** is available with NKP Pro and Ultimate licenses.

1.  Head to your project in the `Default Workspace`
    
    ![Open project](/cloudnative/assets/open_project.5889bf31.png)
    
2.  In the _Clusters_ page on your project, click the dots at the right of _workload01_ and open `Kubernetes`
    
    ![Open Kubernetes dashboard](/cloudnative/assets/open_k8s_dashboard.efdcbc6c.png)
    
    accept the self-signed certificate.
    
    re-authenticate
    
    Dashboards are cluster-scoped, so your authentication cookie in your browser doesn't exist for the _workload01_ external IP address yet.
    
3.  Change the namespace to yours, _user`##`_. You'll see there are no resources yet
    
    ![Select namespace](/cloudnative/assets/k8s_dashboard_ns.03dd6ac1.gif)
    
4.  Click `Storage Classes` on the sidebar menu under **Config and Storage** to check the Storage Classes available
    
    ![Storage Classes](/cloudnative/assets/k8s_dashboard_sc.3bb5f540.png)
    
    Any NKP cluster deployed on Nutanix gets the `nutanix-volume` StorageClass, enabling the deployment of stateful applications out-of-the-box.
    
    Note
    
    The _nutanix-files_ StorageClass is not created during cluster deployment.
    
    You can see one because it has been staged in advance for your next lab.
    
5.  Click the `nutanix-volume` StorageClass and take a look to:
    
    -   **2** - This annotation sets the storage class as default in case a user doesn't set a StorageClass value in their persistent volume request
        
    -   **3** - Settings specifics to the Nutanix CSI driver for Volumes
        
    -   **4** - Persistent volumes using this storage class
        
    
    ![nutanix-volume storage class](/cloudnative/assets/k8s_dashboard_nutanix-volume.9da74e0a.png)