# NKP Advanced Hands-on Lab

Note

-   Expected lab duration: 15 minutes

# [#](#persistent-storage) Persistent storage Lab

When creating an NKP cluster on Nutanix, the Nutanix Container Storage Interface (CSI) Driver for Kubernetes gets deployed by default.

With Nutanix CSI you can:

-   Provide persistent storage to your containers
    
-   Leverage PVC resources to consume dynamically Nutanix storage
    
-   With Files storage classes, applications on multiple pods can access the same storage, and also have the benefit of multi-pod read and write access (ReadWriteMany)
    

To practice with stateful workloads, for this lab we'll be deploying the well-known and broadly adopted WordPress CMS. There are two components, the frontend (UI) based on PHP, and the backend (database) based on MySQL.

-   For MySQL, you'll use block storage with Nutanix Volumes
    
-   For WordPress, you'll use file storage with Nutanix Files
    

![WordPress diagram](/cloudnative/assets/wordpress_diagram.59a9a0b6.png)