# NKP Advanced Hands-on Lab

# [#](#access-the-application) Access the Application

Once the application has been successfully deployed using GitOps, the next step is to access and interact with it.

1.  Navigate to the `Clusters` tab within your Project.
    
2.  On the **workload01** cluster, click on the three dots on the far right and select `Kubernetes`. This will open the built-in Kubernetes dashboard for your cluster.
    
    ![boutique k8s dashboard](/cloudnative/assets/k8s_dashboard.ebdad889.png)
    
3.  In the Kubernetes dashboard, change the namespace from default to your Project's namespace (**user##**).
    
4.  On the `Workloads` section, verify that the online boutique application pods are running, along with other resources. There will be a total of 12 deployments created, 11 for the individual microservices and one for the redis service that will store the shopping cart items.
    
    ![boutique k8s gif](/cloudnative/assets/boutique_app.ca253528.gif)
    
5.  The application is exposed using a LoadBalancer IP address allocated by NKP's built-in load balancer MetalLB.
    
    Note
    
    For a refresher on MetalLB, please visit this [Chapter](/cloudnative/fundamentals/expose-app-prod/loadbalancer.html).
    
6.  Finally, access the boutique store straight from the Kubernetes dashboard as well. Navigate to `Services` and click on the URL for the `frontend-external` service.
    
    ![frontend](/cloudnative/assets/frontend.b677a280.png)
    

---

You should see the online boutique frontend displaying product listings and other features. Now get your shopping cart loaded and treat yourself (virtually)!

![frontend boutique](/cloudnative/assets/frontend_boutique.ce7f8937.png)