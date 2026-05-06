# NKP Advanced Hands-on Lab

# [#](#block-storage-with-nutanix-volumes) Block storage with Nutanix Volumes

Block storage allows ReadWriteOnce (RWO) and ReadOnlyMany (ROX). The persistent volume can be mounted as read-write by a single node.

You'll use block storage from Nutanix Volumes for your MySQL database.

#### [#](#create-a-mysql-database-in-kubernetes) Create a MySQL database in Kubernetes

Reminder

We are deploying the WordPress application on the **workload01** cluster.

1.  On your Kubernetes dashboard, click the plus button at the top right, paste the manifest below, and click _Upload_
    
    ![Create MySQL](/cloudnative/assets/k8s_dashboard_mysql.666d915b.png)
    
    Apply the manifest as it is
    
    ```
    apiVersion: v1
    kind: Secret
    metadata:
      name: mysql-pass
    type: Opaque
    stringData:
      password: nutanix/4u
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: wordpress-mysql
      labels:
        app: wordpress
    spec:
      ports:
        - port: 3306
      selector:
        app: wordpress
        tier: mysql
      clusterIP: None
    ---
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mysql-pv-claim
      labels:
        app: wordpress
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 20Gi
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: wordpress-mysql
      labels:
        app: wordpress
    spec:
      selector:
        matchLabels:
          app: wordpress
          tier: mysql
      strategy:
        type: Recreate
      template:
        metadata:
          labels:
            app: wordpress
            tier: mysql
        spec:
          containers:
            - image: mysql:8.0
              name: mysql
              env:
                - name: MYSQL_ROOT_PASSWORD
                  valueFrom:
                    secretKeyRef:
                      name: mysql-pass
                      key: password
                - name: MYSQL_DATABASE
                  value: wordpress
                - name: MYSQL_USER
                  value: wordpress
                - name: MYSQL_PASSWORD
                  valueFrom:
                    secretKeyRef:
                      name: mysql-pass
                      key: password
              ports:
                - containerPort: 3306
                  name: mysql
              volumeMounts:
                - name: mysql-persistent-storage
                  mountPath: /var/lib/mysql
          volumes:
            - name: mysql-persistent-storage
              persistentVolumeClaim:
                claimName: mysql-pv-claim
    ```
    
2.  Wait for a minute until your MySQL components in _Workload Status_ are green. In the meantime you can read the explanation of the previous manifest
    
    (Optional) Explanation of the MySQL manifest
    
    -   **1-7** Creates a secret for MySQL with the password `nutanix/4u`
    -   **9-21** Creates a headless service (_clusterIP: None_) to expose MySQL on port 3306 internally in the Kubernetes cluster
    -   **23-34** Creates a persistent volume of 20Gi to store the database (the _default_ StorageClass will catch the request)
    -   **36-82** Deploys MySQL version 8.0 using the persistent storage created and passing the password as an environment variable from the secret
    
    ![MySQL status](/cloudnative/assets/k8s_dashboard_mysql_status.2470fe92.png)
    
3.  Click the _Persistent Volume Claims_ option on the sidebar menu to see the persistent volume created for MySQL
    
    ![MySQL PVC](/cloudnative/assets/k8s_dashboard_mysql_pvc.9b901e30.gif)
    
    Pro tip
    
    A block storage PVC is a Volume Group in Nutanix.
    
    ![Block PVCs in PC](/cloudnative/assets/pc_pvcs.477b0625.png)
    

🎉 Congratulations! You have deployed your first stateful application, a MySQL database.