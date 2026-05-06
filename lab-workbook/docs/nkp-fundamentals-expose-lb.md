# NKP Advanced Hands-on Lab

# [#](#load-balancer) Load Balancer

**MetalLB** is a Kubernetes load-balancer (L4 of the OSI model) implementation for self-hosted environments. Some of the benefits of this integration are:

-   **Automatic External IP Allocation**. MetalLB assigns external IPs to LoadBalancer Services automatically, eliminating the need for manual IP assignment
    
-   **High Availability**. Supports multiple nodes for load balancing
    
-   **Seamless Kubernetes Integration**. Doesn't require changes to application manifests or workflows
    

Did You Know?

**MetalLB** is included with all tiers of NKP

#### [#](#looking-at-metallb-configuration) Looking at MetalLB configuration

1.  Let's check what IP address pool is configured. These are the IPs that MetalLB can use to dynamically assign for service requests of type _LoadBalancer_
    
    -   command
    -   output (example)
    
    ```
    kubectl --namespace metallb-system get ipaddresspool
    ```
    
    ```
    NAME      AUTO ASSIGN   AVOID BUGGY IPS   ADDRESSES
    metallb   true          false              ["10.38.30.16-10.38.30.16","10.38.30.39-10.38.30.58"]
    ```
    
    -   The first "range" with a single IP ending on _.16_ was provided during cluster creation and gets assigned to the Traefik Ingress controller
    -   The second range was staged after cluster creation. It supports your next task with exposing the NGINX web server

#### [#](#updating-our-service-to-loadbalancer) Updating our Service to LoadBalancer

Our simple application service is of type _NodePort_. We want to migrate this to type `LoadBalancer`. There are different ways to accomplish this, but the best option is to update the existing service to avoid disruptions or extra configurations.

(Optional) Explanation different migration options

-   Creating a new service of type `LoadBalancer` in addition to the previously created. The downside is that you'll be consuming an additional dynamic port in the nodes
    
-   Deleting and re-creating the service but this time setting the type to `LoadBalancer`. In production this approach would have downtime
    
-   Updating the existing service to type `LoadBalancer`. The dynamic port allocated to `NodePort` will remain the same, and in addition an IP from the load balancing pool will be assigned. This method avoids disruptions in production and it is the one you'll be performing
    

1.  List your existing service. Make sure you update `##` with your user number
    
    -   command
    -   example
    -   output (example)
    
    ```
    kubectl get service user##-nkp-simple-app
    ```
    
    ```
    kubectl get service user01-nkp-simple-app
    ```
    
    ```
    NAME                                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    user01-nkp-simple-app                                     NodePort    10.99.58.51     <none>        80:31347/TCP   112m
    ```
    
    In the example, the service `user##-nkp-simple-app` is listening on port `31347` across all the nodes. There isn't an `EXTERNAL-IP` assigned.
    
2.  Let's go ahead and update our service using `PATCH` to change the type to `LoadBalancer`. Make sure you update `##` with your user number
    
    -   command
    -   example
    -   example (output)
    
    ```
    kubectl patch service user##-nkp-simple-app \
        -p '{"spec": {"type": "LoadBalancer"}}'
    ```
    
    ```
    kubectl patch service user01-nkp-simple-app \
        -p '{"spec": {"type": "LoadBalancer"}}'
    ```
    
    ```
    service/user01-nkp-simple-app patched
    ```
    
3.  Let see what IP address from the pool has been assigned in the `EXTERNAL-IP` column. Make sure you update `##` with your user number
    
    -   command
    -   example
    -   output (example)
    
    ```
    kubectl get service user##-nkp-simple-app
    ```
    
    ```
    kubectl get service user01-nkp-simple-app
    ```
    
    ```
    NAME                    TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
    user01-nkp-simple-app   LoadBalancer   10.99.58.51   10.38.30.39   80:31347/TCP   161m
    ```
    
    Pro tip
    
    A Load Balancer Controller in Kubernetes is typically **cluster-scoped**, meaning that it will listen for any request coming from any namespace requesting a Service of type `LoadBalancer`.
    
4.  Using _curl_ on your terminal or your web browser, open the page using the load balancer IP address, and the original backend container port `80`
    
    -   command
    -   example
    -   output (example)
    
    ```
    curl http://<EXTERNAL-IP>:<CONTAINER_PORT>
    ```
    
    ```
    curl http://10.38.30.39
    ```
    
    ```
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
    html { color-scheme: light dark; }
    body { width: 35em; margin: 0 auto;
    font-family: Tahoma, Verdana, Arial, sans-serif; }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>
    
    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>
    
    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>
    ```
    
    ![Nginx Chrome](/cloudnative/assets/nginx_chrome.2cddfb3c.png)
    

(Optional) Diagram representing of what you just did

![LoadBalancer diagram](/cloudnative/assets/loadbalancer_diagram.ddf76924.png)