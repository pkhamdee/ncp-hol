# NKP Advanced Hands-on Lab

# [#](#nkp-projects) NKP Projects

Projects align with "soft" multi-tenancy enabling to share Kubernetes clusters among several teams, providing them self-service.

When a Project is created, NKP creates a federated namespace that is propagated to the Kubernetes clusters associated with this Project. Federation in this context means that a common configuration is pushed out from a central location (NKP) to all Kubernetes clusters, or a pre-defined subset group, under NKP management.

**Project Namespaces**

A Project Namespace is a NKP specific concept, it isolates configurations across clusters and are created on all clusters matching the project labels.

In this lab you will create your own project to use with the upcoming labs.

#### [#](#creating-a-nkp-project) Creating a NKP Project

1.  On the _Default Workspace_, choose _Projects_ on the sidebar menu and click `+ Create Project`
    
    Note
    
    If there are projects created, the `+ Create Project` blue button is at the top right of the screen
    
    ![Create a project](/cloudnative/assets/project_create_01.d31f5329.png)
    
2.  Use the following settings. Make sure you update `##` with your user number:
    
    -   Project Name: **user`##`**
        
    -   ID / Namespace: **user`##`** (same as _Project Name_)
        
    -   Clusters: **Manually Selected Clusters**
        
    -   Selected Cluster: **workload01** (only)
        
    
    ![Project settings](/cloudnative/assets/project_create_02.39ccb846.png)
    
3.  Click **Continue to Project**
    
    ![Continue to Project](/cloudnative/assets/project_create_03.f2df98a8.png)