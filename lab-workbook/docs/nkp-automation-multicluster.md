# NKP Advanced Hands-on Lab

Note

-   Expected lab duration: 6 minutes

# [#](#automate-multicluster-app-deployment) Automate Multicluster App Deployment Lab

Previously, we have used GitOps to automate application deployment on one cluster. In this section, let's see how to scale this automation across a fleet of clusters!

1.  Head back to the NKP console and click on the Projects tab in the left-hand navigation pane.
    
2.  View the project corresponding to your user. You will see that the cluster `workload01` is assigned to this project. This namespace and project are federated exclusively to this single cluster. The example below is for **user16**.
    
3.  Edit the Project to modify the project configuration.
    
    ![edit project](/cloudnative/assets/edit_project.37d0cfa6.png)
    
4.  Notice that the clusters are currently assigned manually through explicit selection. Switch the assignment method to Dynamic. With dynamic assignment, NKP uses predefined labels to automatically assign clusters to the project. For example, you can configure a Project to include all clusters labeled with `region: us-west` and `team: devops`. As new clusters matching these labels are created — whether on vSphere, AWS, or EKS — NKP automatically adds them to the Project.
    
    Similarly, if labels are removed or changed, the corresponding clusters are dynamically excluded. This ensures policies and configurations are applied automatically without requiring manual updates.
    
    ![Dynamic selection](/cloudnative/assets/dynamic_select.82f4d127.gif)
    
5.  Make sure you add the following label on the `Clusters` section:
    
    -   Key:
        
        ```
        infraId
        ```
        
    -   Value:
        
        ```
        pc
        ```
        
    
    You will notice that the cluster `workload02` is immediately selected since it already has this label. If it isn't, ensure you typed _infraId_ with capital `I`
    
    ![Add label](/cloudnative/assets/add_label.51db0425.gif)
    
6.  Click Save. Navigate back to the Clusters page, where you’ll now see both `workload01` and `workload02` assigned to your Project.
    
    ![two cluster](/cloudnative/assets/two_cluster.db371afc.png)
    
7.  Let's verify the app is deployed to the `workload02` cluster. Navigate to the Clusters page in the NKP console. Choose the Kubernetes Dashboard for the `workload02` cluster.
    
    Log in using your credentials, same as earlier.
    
    ![k8s dashboard cluster2](/cloudnative/assets/k8s_dashboard_cluster2.aee837a6.png)
    
8.  Once in the Kubernetes dashboard, switch to the appropriate Project namespace (**user##**) where the application is deployed. Verify that the application is running by observing the pods, which should display a creation timestamp from a few minutes ago.
    
    ![boutique cluster2](/cloudnative/assets/boutique_cluster2.12984522.gif)
    

---

If a new cluster (or 50 clusters) with the label **`infraId: pc`** is added, the boutique application will automatically deploy to that cluster(s), ensuring seamless scaling without manual intervention!