# NKP Advanced Hands-on Lab

# [#](#using-kubectl-to-create-a-deployment) Using kubectl to Create a Deployment

Let’s deploy our first app on Kubernetes with the `kubectl create deployment` command.

1.  We need to provide the deployment name and app image location. Back in the VS Code terminal, run the following command updating `##` first with your user number
    
    -   command
    -   example
    -   output (example)
    
    ```
    kubectl create deployment user##-nkp-simple-app --image=nginx:1.27
    ```
    
    ```
    kubectl create deployment user01-nkp-simple-app --image=nginx:1.27
    ```
    
    ```
    deployment.apps/user01-nkp-simple-app created
    ```
    
    🎉 Great! You just deployed your first application by creating a deployment.
    
    (Optional) Explanation of deployment creation
    
    This performed a few things for you:
    
    -   searched for a suitable node where an instance of the application could be run
    -   scheduled the application to run on that Node
    -   configured the cluster to reschedule the instance on a new Node when needed
    
2.  Go ahead and list the deployments:
    
    -   command
    -   output (example)
    
    ```
    kubectl get deployments
    ```
    
    ```
    NAME                                                      READY   UP-TO-DATE   AVAILABLE   AGE
    cluster-autoscaler-0193785b-4589-7210-9427-7709ece7adfc   1/1     1            1           45h
    user01-nkp-simple-app                                     1/1     1            1           30s
    ```
    
    We see that the `user##-nkp-simple-app` deployment is running a single instance of the app.
    

#### [#](#view-the-app) View the app

We will cover other options on how to expose your application outside the Kubernetes cluster on the next lab.

(Optional) View the app during development

Pods that are running inside Kubernetes are running on a private, isolated network. By default they are visible from other pods and services within the same Kubernetes cluster, but not outside that network.

The `kubectl proxy` command can create a proxy that will forward communications into the cluster-wide, private network. We won't be using this option because this is only useful for development.