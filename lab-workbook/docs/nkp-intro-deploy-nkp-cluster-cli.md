# NKP Advanced Hands-on Lab

# [#](#argument-based-cli-installation) Argument-based CLI Installation

1.  Create a `.env` file on VS Code
    
    ![VS Code create .env](/cloudnative/assets/ide-env-file.774181ba.gif)
    
2.  Paste the code block below and update **only** the values `highlighted`. You can find the values in your lab page, ask your instructor, or expand the following table:
    
    Get the IP addresses from your instructor or expand the section below to lookup your IP.
    
    Assigned IPs
    
    Important! Your MetalLB "range" is just one IP. Since you won't be using this cluster, there is no need for additional IPs. You must still input the value on the format `#.#.#.<start>-#.#.#.<end>` even if it is a single IP.
    
    User
    
    CONTROL\_PLANE\_ENDPOINT\_IP
    
    LB\_IP\_RANGE
    
    adminuser01
    
    #.#.#.**134**
    
    #.#.#.**135**\-#.#.#.**135**
    
    adminuser02
    
    #.#.#.**136**
    
    #.#.#.**137**\-#.#.#.**137**
    
    adminuser03
    
    #.#.#.**138**
    
    #.#.#.**139**\-#.#.#.**139**
    
    adminuser04
    
    #.#.#.**140**
    
    #.#.#.**141**\-#.#.#.**141**
    
    adminuser05
    
    #.#.#.**142**
    
    #.#.#.**143**\-#.#.#.**143**
    
    adminuser06
    
    #.#.#.**144**
    
    #.#.#.**145**\-#.#.#.**145**
    
    adminuser07
    
    #.#.#.**146**
    
    #.#.#.**147**\-#.#.#.**147**
    
    adminuser08
    
    #.#.#.**148**
    
    #.#.#.**149**\-#.#.#.**149**
    
    adminuser09
    
    #.#.#.**150**
    
    #.#.#.**151**\-#.#.#.**151**
    
    adminuser10
    
    #.#.#.**152**
    
    #.#.#.**153**\-#.#.#.**153**
    
    adminuser11
    
    #.#.#.**154**
    
    #.#.#.**155**\-#.#.#.**155**
    
    adminuser12
    
    #.#.#.**156**
    
    #.#.#.**157**\-#.#.#.**157**
    
    adminuser13
    
    #.#.#.**158**
    
    #.#.#.**159**\-#.#.#.**159**
    
    adminuser14
    
    #.#.#.**160**
    
    #.#.#.**161**\-#.#.#.**161**
    
    adminuser15
    
    #.#.#.**162**
    
    #.#.#.**163**\-#.#.#.**163**
    
    adminuser16
    
    #.#.#.**164**
    
    #.#.#.**165**\-#.#.#.**165**
    
    adminuser17
    
    #.#.#.**166**
    
    #.#.#.**167**\-#.#.#.**167**
    
    adminuser18
    
    #.#.#.**168**
    
    #.#.#.**169**\-#.#.#.**169**
    
    adminuser19
    
    #.#.#.**170**
    
    #.#.#.**171**\-#.#.#.**171**
    
    adminuser20
    
    #.#.#.**172**
    
    #.#.#.**173**\-#.#.#.**173**
    
    Pro tip
    
    1.  Do not highlight line by line for copy/pasting
    2.  Use the copy icon and paste the entire code block in the .env file
    3.  Update the values in the .env file  
          
        
    
    -   env
    -   example
    
    ```
    # NKP version to install
    # Do not change it
    export NKP_VERSION=2.17.0
    
    # NKP cluster name.
    # When using NKP Pro/Ultimate, this name is used to generate the license key
    export CLUSTER_NAME=user##-nkp
    
    # Prism Central username
    export NUTANIX_USER='adminuser##@ntnxlab.local'
    
    # Prism Central Password
    # Keep the password enclosed between single quotes - Ex: 'password'
    export NUTANIX_PASSWORD=''
    
    # Prism Central IP address - Ex: 10.38.30.7
    export NUTANIX_ENDPOINT=#.#.#.7
    
    # Prism Central port (default: 9440)
    # Do not change it
    export NUTANIX_PORT=9440
    
    # Kubernetes VIP. Must be in the same subnet as the VMs
    # Check the table to find your IP
    export CONTROL_PLANE_ENDPOINT_IP=#.#.#.#
    
    # Load balancer IP range. Format: <first_ip>-<last_ip>
    # Check the table to find your IP
    export LB_IP_RANGE=#.#.#.#-#.#.#.#
    
    # NKP Rocky image name
    # Do not change it
    export NUTANIX_MACHINE_TEMPLATE_IMAGE_NAME=nkp-rocky-9.6-release-cis-1.34.1-20251219225533.qcow2
    
    # Prism Element cluster name - Ex: PHX-POC207
    export NUTANIX_PRISM_ELEMENT_CLUSTER_NAME=
    
    # NKP cluster subnet
    # Do not change it
    export NUTANIX_SUBNET_NAME=secondary
    
    # Prism storage container
    # Do not change it
    export NUTANIX_STORAGE_CONTAINER_NAME=SelfServiceContainer
    
    # Required on Nutanix HPOC
    # Do not change it
    export REGISTRY_MIRROR_URL=registry.nutanixdemo.com/bootcamps
    ```
    
      
      
      
      
      
      
    
      
      
    
      
      
      
    
      
      
    
      
      
      
      
      
      
      
    
      
      
      
    
      
      
      
      
      
      
    
      
      
      
      
      
      
      
      
      
      
      
      
    
    ```
    export NKP_VERSION=2.17.0
    export CLUSTER_NAME=user01-nkp
    export NUTANIX_USER='adminuser01@ntnxlab.local'
    export NUTANIX_PASSWORD='password'
    export NUTANIX_ENDPOINT=10.38.30.7
    export NUTANIX_PORT=9440
    export LB_IP_RANGE=10.38.30.135-10.38.30.135
    export CONTROL_PLANE_ENDPOINT_IP=10.38.30.134
    export NUTANIX_MACHINE_TEMPLATE_IMAGE_NAME=nkp-rocky-9.6-release-cis-1.34.1-20251219225533.qcow2
    export NUTANIX_PRISM_ELEMENT_CLUSTER_NAME=PHX-POC000
    export NUTANIX_SUBNET_NAME=secondary
    export NUTANIX_STORAGE_CONTAINER_NAME=SelfServiceContainer
    export REGISTRY_MIRROR_URL=registry.nutanixdemo.com/bootcamps
    ```
    
3.  Load the environment variables with the following command
    
    -   command
    
    ```
    source ~/.env
    ```
    
4.  Go ahead and create your first management cluster
    
    -   command
    -   output
    
    ```
    nkp create cluster nutanix -c $CLUSTER_NAME \
        --bootstrap-cluster-image $REGISTRY_MIRROR_URL/mesosphere/konvoy-bootstrap:v$NKP_VERSION \
        --endpoint https://$NUTANIX_ENDPOINT:$NUTANIX_PORT \
        --insecure \
        --control-plane-endpoint-ip $CONTROL_PLANE_ENDPOINT_IP \
        --kubernetes-service-load-balancer-ip-range $LB_IP_RANGE \
        --control-plane-vm-image $NUTANIX_MACHINE_TEMPLATE_IMAGE_NAME \
        --control-plane-prism-element-cluster $NUTANIX_PRISM_ELEMENT_CLUSTER_NAME \
        --control-plane-subnets $NUTANIX_SUBNET_NAME \
        --worker-vm-image $NUTANIX_MACHINE_TEMPLATE_IMAGE_NAME \
        --worker-prism-element-cluster $NUTANIX_PRISM_ELEMENT_CLUSTER_NAME \
        --worker-subnets $NUTANIX_SUBNET_NAME \
        --csi-storage-container $NUTANIX_STORAGE_CONTAINER_NAME \
        --registry-mirror-url https://$REGISTRY_MIRROR_URL \
        --skip-preflight-checks=Registry \
        --control-plane-replicas 1 \
        --control-plane-memory 6 \
        --worker-replicas 2 \
        --worker-vcpus 4 \
        --worker-memory 6 \
        --self-managed
    ```
    
    ```
    ✓ Creating a bootstrap cluster
    ⠈⡱ Initializing new CAPI components
    [...]
    ```
    
    Note
    
    -   NKP cluster creation can take up to 30 minutes
        
    -   The cluster you are deploying uses the minimum possible resources to showcase the process; do not use this in production. You can see the arguments from line **15** to **19**.
        
    
    **Don't wait for your cluster to complete**. You won't use it for the upcoming labs, instead we use a shared multi-cluster setup.
    

(Optional) Explanation NKP cluster creation process

NKP uses an open-source project called Cluster API (CAPI), a Kubernetes subproject focused on providing declarative APIs and tooling to simplify provisioning, upgrading, and operating multiple Kubernetes clusters. In simple terms, CAPI deploys and manages Kubernetes clusters from Kubernetes itself.

When you run the NKP `create cluster` command, it deploys a small bootstrap Kubernetes cluster in your Admin VM using `kubeadm` and the Docker Engine you installed during the Admin VM creation.

Then, the CAPI components (infrastructure providers) are installed in the Kubernetes cluster. With the inputs you provided with the environment variables, the infrastructure provider for Nutanix is used to create the virtual machines in the Nutanix cluster and, from there, create a Kubernetes cluster (self-managed / management cluster)

Once that cluster is ready, the CAPI components are migrated from the bootstrap cluster to this first NKP cluster, becoming the NKP management cluster (it can also run workloads when self-managed - single cluster). At this stage, the bootstrap cluster is deleted.

With the CAPI controllers migrated, the installation process for the NKP platform applications starts. You can recognize this in your terminal when you see the `Starting kommander installation` message.

After a few minutes, you'll see a message with the cluster created successfully and the command to access the NKP cluster dashboard.

(Optional) Did you know NKP includes a prompt-based installation method?

![TUI-based method](/cloudnative/assets/tui-based.5759fd86.png)

We didn't use this simplified method because on busy environments like Nutanix HPOC, with a hundred of requests per minute to Docker Hub, you must use an internal container registry mirror to overcome the Docker Hub rate limits for pulling container images. Providing a registry mirror is only available using argument-based installation.