# NKP Advanced Hands-on Lab

# [#](#file-storage-with-nutanix-files) File storage with Nutanix Files

Frontends are scale-out tiers, they need access to the same data. WordPress as CMS needs a place to store documents, images, plugins, and any other static content. We shouldn't use block-based PVC because other pods would not be able to access this persistent volume.

We need shared storage, a ReadWriteMany persistent volume! Nutanix Files is the answer 🚀.

Note

The cluster already has the storage class for Nutanix Files (covered during the [StorageClass with Nutanix CSI](/cloudnative/fundamentals/persistent-storage/storage-class.html) section)

#### [#](#create-a-stateful-wordpress-frontend-tier) Create a Stateful WordPress frontend tier

1.  On your Kubernetes dashboard, make sure you are in your namespace, click the plus button at the top right, paste the manifest below, and click _Upload_
    
    ![Create WordPress](/cloudnative/assets/k8s_dashboard_wordpress.1a4327b3.png)
    
    Apply the manifest as it is
    
    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: wordpress
      labels:
        app: wordpress
    spec:
      ports:
        - port: 80
      selector:
        app: wordpress
        tier: frontend
      clusterIP: None
    ---
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: wp-pv-claim
      labels:
        app: wordpress
    spec:
      storageClassName: nutanix-files
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: wordpress
      labels:
        app: wordpress
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: wordpress
          tier: frontend
      strategy:
        type: Recreate
      template:
        metadata:
          labels:
            app: wordpress
            tier: frontend
        spec:
          containers:
          - image: wordpress:apache
            name: wordpress
            env:
            - name: WORDPRESS_DB_HOST
              value: wordpress-mysql
            - name: WORDPRESS_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-pass
                  key: password
            - name: WORDPRESS_DB_USER
              value: wordpress
            ports:
            - containerPort: 80
              name: wordpress
            volumeMounts:
            - name: wordpress-persistent-storage
              mountPath: /var/www/html
          volumes:
          - name: wordpress-persistent-storage
            persistentVolumeClaim:
              claimName: wp-pv-claim
    ```
    
2.  Wait for a minute until your WordPress deployment in _Deployments_ is green. In the meantime you can read the explanation of the previous manifest
    
    (Optional) Explanation of the WordPress manifest
    
    -   **1-13** Creates a headless service to expose WordPress on port 80 internally in the Kubernetes cluster
    -   **15-27** Creates a `ReadWriteMany` persistent volume of 20Gi to store the WordPress static content
    -   **29-71** Deploys two replicas (**36**) of WordPress version 6.2.1 using the persistent storage created, and passes the MySQL address (**53-54**) and password as an environment variable from a secret (**55-59**)
    
    Pro tip
    
    A file storage PVC is an NFS share in Nutanix Files.
    
    ![Block PVCs in PC](/cloudnative/assets/files_shares.46da30f8.png)
    

#### [#](#accessing-wordpress) Accessing WordPress

The WordPress application isn't externally accessible yet. Let's use an _Ingress_ resource for this purpose.

1.  On the sidebar menu, go to `Ingresses` under the **Service** section. Ensure your are on your namespace. You must not have any ingress yet
    
    ![WordPress ingress](/cloudnative/assets/k8s_dashboard_wordpress_ingress.76148cd8.png)
    
2.  Apply the following manifest updating first the `highlighted` line **8** with your user number after wordpress and ingress IP.
    
    Pro tip
    
    Your Ingress IP ends on **.19**. Ex: 10.38.30.**19**
    
    This is the ingress service on the **workload01** cluster.
    
    ![WordPress ingress manifest](/cloudnative/assets/k8s_dashboard_wordpress_ingress_manifest.c9b5e96c.png)
    
    -    manifest
    -   example
    
    ```
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: wordpress
    spec:
      ingressClassName: kommander-traefik
      rules:
        - host: wordpress##.<ingress_lb_ip>.sslip.io
          http:
            paths:
              - path: /
                pathType: Prefix
                backend:
                  service:
                    name: wordpress
                    port:
                      number: 80
    ```
    
      
      
      
      
      
      
      
    
      
      
      
      
      
      
      
      
      
    
    ```
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: wordpress
    spec:
      ingressClassName: kommander-traefik
      rules:
        - host: wordpress01.10.38.30.19.sslip.io
          http:
            paths:
              - path: /
                pathType: Prefix
                backend:
                  service:
                    name: wordpress
                    port:
                      number: 80
    ```
    
      
      
      
      
      
      
      
    
      
      
      
      
      
      
      
      
      
    
3.  Scroll down to the Ingresses section and open the URL in the _Hosts_ column.
    
    accept the self-signed certificate.
    
    Do not complete WordPress installation.
    
    ![Open WordPress URL](/cloudnative/assets/wordpress_url.649161c2.gif)
    

🎉 Congratulations! You just got an entire WordPress application with two different persistent storage, and external access with an ingress ready to go.