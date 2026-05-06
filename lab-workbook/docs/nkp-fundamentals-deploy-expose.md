# NKP Advanced Hands-on Lab

# [#](#using-a-service-to-expose-your-app) Using a Service to Expose Your App

A Service in Kubernetes is an abstraction that defines a logical set of Pods and a policy for routing traffic to your applications.

Services can be exposed in different ways by specifying a `type` in the spec of the Service. Most common services are:

-   _ClusterIP_ (default) - Exposes the Service on an internal IP in the cluster. Service only reachable from within the cluster.
    
-   _NodePort_ - Exposes the Service on the same port of each selected Node in the cluster using NAT. Service accessible from outside the cluster using `<NodeIP>:<NodePort>`. Superset of ClusterIP.
    
-   _LoadBalancer_ - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
    

#### [#](#creating-a-nodeport-service) Creating a NodePort Service

A NodePort service is the most used service type for users getting started with Kubernetes when they don't have a load balancing service.

1.  We'll use the expose command with NodePort as parameter to create a new service. Make sure you update `##` with your user number
    
    -   command
    -   example
    -   output
    
    ```
    kubectl expose deployment/user##-nkp-simple-app --type="NodePort" --port 80
    ```
    
    ```
    kubectl expose deployment/user01-nkp-simple-app --type="NodePort" --port 80
    ```
    
    ```
    service/user01-nkp-simple-app exposed
    ```
    
2.  Run the `get services` subcommand:
    
    -   command
    -   output (example)
    
    ```
    kubectl get services
    ```
    
    ```
    NAME                                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
    cluster-autoscaler-0193785b-4589-7210-9427-7709ece7adfc   ClusterIP   10.108.171.72    <none>        8085/TCP         2d5h
    kubernetes                                                ClusterIP   10.96.0.1        <none>        443/TCP          2d5h
    user01-nkp-simple-app                                     NodePort    10.111.180.214   <none>        80:31347/TCP     46s
    ```
    
    We have now a running service called `user##-nkp-simple-app`. Here we see that the service received a unique cluster-IP, the internal container port, and an external node port.
    
3.  (Optional) To see additional details for this service you can run the `describe service` subcommand:
    
    -   command
    -   example
    -   output (example)
    
    ```
    kubectl describe services/user##-nkp-simple-app
    ```
    
    ```
    kubectl describe services/user01-nkp-simple-app
    ```
    
    ```
    Name:                     user01-nkp-simple-app
    Namespace:                default
    Labels:                   app=user01-nkp-simple-app
    Annotations:              <none>
    Selector:                 app=user01-nkp-simple-app
    Type:                     NodePort
    IP Family Policy:         SingleStack
    IP Families:              IPv4
    IP:                       10.96.29.176
    IPs:                      10.96.29.176
    Port:                     <unset>  80/TCP
    TargetPort:               80/TCP
    NodePort:                 <unset>  31347/TCP
    Endpoints:                192.168.5.51:80
    Session Affinity:         None
    External Traffic Policy:  Cluster
    Internal Traffic Policy:  Cluster
    Events:                   <none>
    ```
    
4.  Let's create an environment variable called _NODE\_PORT_ that has the value of the Node port assigned. Make sure you update `##` with your user number:
    
    -   command
    -   example
    -   output (example)
    
    ```
    export NODE_PORT="$(kubectl get services/user##-nkp-simple-app -o go-template='{{(index .spec.ports 0).nodePort}}')"
    echo "NODE_PORT=$NODE_PORT"
    ```
    
    ```
    export NODE_PORT="$(kubectl get services/user01-nkp-simple-app -o go-template='{{(index .spec.ports 0).nodePort}}')"
    echo "NODE_PORT=$NODE_PORT"
    ```
    
    ```
    NODE_PORT=31347
    ```
    
5.  To test the app, first we need to know the IP addresses for our Nodes. Run the following command:
    
    -   command
    -   output (example)
    
    ```
    kubectl get nodes -o wide
    ```
    
    ```
    NAME                         STATUS   ROLES           AGE   VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
    nkp-brfn6-68ppw              Ready    control-plane   12h   v1.34.1   10.38.30.81   <none>        Ubuntu 24.04.2 LTS   6.8.0-64-generic   containerd://1.7.29-d2iq.1
    nkp-brfn6-rsfcq              Ready    control-plane   12h   v1.34.1   10.38.30.80   <none>        Ubuntu 24.04.2 LTS   6.8.0-64-generic   containerd://1.7.29-d2iq.1
    nkp-brfn6-xkclb              Ready    control-plane   12h   v1.34.1   10.38.30.116   <none>        Ubuntu 24.04.2 LTS   6.8.0-64-generic   containerd://1.7.29-d2iq.1
    nkp-md-0-rbns5-fvdf8-7qfjq   Ready    <none>          12h   v1.34.1   10.38.30.124   <none>        Ubuntu 24.04.2 LTS   6.8.0-64-generic   containerd://1.7.29-d2iq.1
    nkp-md-0-rbns5-fvdf8-l22vp   Ready    <none>          12h   v1.34.1   10.38.30.97   <none>        Ubuntu 24.04.2 LTS   6.8.0-64-generic   containerd://1.7.29-d2iq.1
    nkp-md-0-rbns5-fvdf8-qzbdn   Ready    <none>          12h   v1.34.1   10.38.30.114   <none>        Ubuntu 24.04.2 LTS   6.8.0-64-generic   containerd://1.7.29-d2iq.1
    nkp-md-0-rbns5-fvdf8-vkkpn   Ready    <none>          12h   v1.34.1   10.38.30.79   <none>        Ubuntu 24.04.2 LTS   6.8.0-64-generic   containerd://1.7.29-d2iq.1
    ```
    
6.  Choose **any** of the _INTERNAL-IP_ addresses and run the following command. Make sure you update `<any_node_IP>` with your preferred node IP:
    
    -   command
    -   example
    -   output (example)
    
    ```
    echo http://<any_node_IP>:$NODE_PORT
    ```
    
    ```
    echo http://10.38.30.124:$NODE_PORT
    ```
    
    ```
    http://10.38.30.124:31347
    ```
    
7.  Open the URL you got from the previous command in your browser. You should see the NGINX web server page
    
    ![Nginx Chrome](/cloudnative/assets/nginx_chrome.7d0cd40e.png)
    

(Optional) Diagram representing of what you just did

![NodePort diagram](/cloudnative/assets/nodeport_diagram.d7992df9.png)