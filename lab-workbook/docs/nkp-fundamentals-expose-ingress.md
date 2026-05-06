# NKP Advanced Hands-on Lab

# [#](#ingress) Ingress

Kubernetes Ingress is a resource that manages external or internal access to services within the cluster, typically HTTP and HTTPS traffic. Some of the benefits of this integration are:

-   **Simplified Traffic Management**. Centralizes and simplifies routing configuration compared to managing multiple individual services with LoadBalancer or NodePort types
    
-   **TLS Termination**. Ingress can handle SSL/TLS termination
    
-   **Advanced Routing**. Supports host-based (e.g., `app.example.com`) and path-based (e.g., `/api/`) routing
    

Like it happens with the load balancing service, Kubernetes only provides the Kubernetes Ingress resource API, but not an Ingress controller.

Did You Know?

**Traefik** is included with all tiers of NKP

**Traefik** is a modern, open-source, cloud-native **edge router** and **reverse proxy** designed to simplify managing and deploying microservices. In NKP, Traefik acts as an **Ingress Controller**, dynamically managing Ingress resources and routing traffic to backend services.

#### [#](#looking-at-existing-ingress-resources) Looking at existing Ingress resources

1.  Before creating our Ingress resource for our simple application, let's check all the Ingress resources existing in the cluster
    
    -   command
    -   output (example)
    
    ```
    kubectl get ingresses --all-namespaces
    ```
    
    ```
    NAMESPACE             NAME                        CLASS               HOSTS   ADDRESS         PORTS   AGE
    git-operator-system   git-operator-git            kommander-traefik   *       10.38.30.16   80      42h
    kommander             dex                         <none>              *       10.38.30.16   80      42h
    kommander             dex-k8s-authenticator       <none>              *       10.38.30.16   80      42h
    kommander             kommander-kommander-ui      <none>              *       10.38.30.16   80      42h
    kommander             kube-oidc-proxy             <none>              *       10.38.30.16   80      42h
    kommander             traefik-dashboard           <none>              *       10.38.30.16   80      42h
    kommander             traefik-forward-auth-mgmt   <none>              *       10.38.30.16   80      42h
    ```
    
    From the previous command you can identify services with an ingress resource, and what's the load balancing IP assigned to the Traefik Ingress controller, In this example, `10.38.30.16`.
    

#### [#](#create-ingress-resource-for-our-simple-app) Create Ingress resource for our simple app

1.  Using VS Code, create a new file called `simple-app-ingress.yaml`
    
    Note
    
    Ensure when you click the new file icon, you are under _NUTANIX_ directory and not inside of any of the folders.
    
    ![Create ingress manifest](/cloudnative/assets/ingress_manifest.3e7c3a16.gif)
    
2.  Paste the following and update the `highlighted` lines with:
    
    -   **4, 15** - Your user number. Ex: user`01`\-nkp-simple-app
    -   **8** - Your user number followed by the Ingress IP from the previous step. Ex: user`01`.`10.38.30`.16.sslip.io  
          
        
    
    -    manifest
    -   example
    
    ```
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: user##-nkp-simple-app
    spec:
      ingressClassName: kommander-traefik
      rules:
        - host: user##.<ingress_lb_ip>.sslip.io
          http:
            paths:
              - path: /
                pathType: Prefix
                backend:
                  service:
                    name: user##-nkp-simple-app
                    port:
                      number: 80
    ```
    
      
      
      
    
      
      
      
    
      
      
      
      
      
      
    
      
      
    
    ```
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: user01-nkp-simple-app
    spec:
      ingressClassName: kommander-traefik
      rules:
        - host: user01.10.38.30.16.sslip.io
          http:
            paths:
              - path: /
                pathType: Prefix
                backend:
                  service:
                    name: user01-nkp-simple-app
                    port:
                      number: 80
    ```
    
3.  Apply the manifest with the following command:
    
    -   command
    -   output
    
    ```
    kubectl apply -f simple-app-ingress.yaml
    ```
    
    ```
    ingress.networking.k8s.io/user18-nkp-simple-app created
    ```
    
    Pro tip
    
    An Ingress Controller in Kubernetes is typically **cluster-scoped**, meaning that it will listen for any request coming from any namespace requesting an Ingress that matches the `ingressClassName`.
    
    You can have multiple Ingress Controllers installed. If you are looking on replacing Traefik, the recommendation is to leave Traefik for the NKP platform services and use your deployment for your applications.
    
4.  Using a new tab in your browser, open your app by connecting to the FQDN you gave it on line 8 of your yaml file over https. Ex: https://user**01**.10.38.30.16.sslip.io
    
    accept the self-signed certificate.
    
    ![Nginx Chrome](/cloudnative/assets/nginx_chrome_ingress.45cfa666.png)
    

(Optional) Diagram representing of what you just did

![Ingress diagram](/cloudnative/assets/ingress_diagram.0206ce42.png)