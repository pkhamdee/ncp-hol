# NKP Advanced Hands-on Lab

# [#](#vm-creation) VM Creation

1.  Access Prism Central using _Company ID_ and your lab user _adminuser`##`_. Replace `##` with your user number
    
    ![Prism Central login](/cloudnative/assets/pc_login.35dd78fb.png)
    
2.  Go to the VMs page
    
    ![Prism Central VM page](/cloudnative/assets/vm-menu.159be0ad.png)
    
3.  Click **Create VM**, use the following configuration, and click **Next**
    
    Input required
    
    Replace `##` with your user number
    
    -    name
    -   example
    
    ```
    user##-adminvm
    ```
    
    ```
    user01-adminvm
    ```
    
    -   CPU: **4** | Cores: **1** | Memory: **8**
    
    ![Create VM](/cloudnative/assets/1-create-vm-configuration.8d687dca.png)
    
4.  Click **Attach Disk**, select the NKP Rocky image, and set the _Capacity_. Click **Save** when finished
    
    -   Operation: **Clone from Image**
        
    -   Image: **nkp-rocky-9.6-release-cis-1.34.1-20251219225533.qcow2**
        
    -   Capacity: **128**
        
    
    ![Attach disk](/cloudnative/assets/2-create-vm-resources-disk.5e71e0b7.png)
    
5.  Click **Attach to Subnet** and select the IPAM-enabled subnet. Click **Save** when finished
    
    Subnet: secondary
    
    ![Attach subnet](/cloudnative/assets/2-create-vm-resources-subnet.53b5a64d.png)
    
6.  Confirm the settings and click **Next**
    
    ![Resources summary](/cloudnative/assets/2-create-vm-resources-summary.14fcfbb1.png)
    
7.  Use the Guest Customization feature to inject a cloud-init script for automating dependency installation for the NKP CLI and bootstrapping cluster. Update the `fqdn` value with your username before proceeding. Click **Next** when finished
    
    -   Script Type: Cloud-init (Linux)
        
        Input required
        
        Replace `##` with your user number
        
        -   config
        -   example
        
        ```
        #cloud-config
        fqdn: user## # <-- Update with your user number
        ssh_pwauth: true
        users:
          - name: nutanix
            primary_group: nutanix
            groups: [wheel, docker]
            shell: /bin/bash
            sudo: ALL=(ALL) NOPASSWD:ALL
            lock_passwd: false
        chpasswd:
          expire: false
          users:
          - name: nutanix
            password: nutanix/4u
            type: text
        runcmd:
        - '[ ! -f "/etc/yum.repos.d/nutanix_rocky9.repo" ] || mv -f /etc/yum.repos.d/nutanix_rocky9.repo /etc/yum.repos.d/nutanix_rocky9.repo.disabled'
        - dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
        - dnf -y install git docker-ce docker-ce-cli containerd.io
        - systemctl --now enable docker
        - usermod -aG docker nutanix
        - 'curl -Lo /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
        - chmod +x /usr/local/bin/kubectl
        - 'curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash'
        - 'su - nutanix -c "curl -fsSL http://10.42.194.11/workshop_staging/tradeshows/experimental/nkp-bootcamp/install-tools.sh | bash"'
        - eject
        ```
        
          
        
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
        
        ```
        #cloud-config
        fqdn: user01 # <-- Update with your user number
        ssh_pwauth: true
        users:
          - name: nutanix
            primary_group: nutanix
            groups: [wheel, docker]
            shell: /bin/bash
            sudo: ALL=(ALL) NOPASSWD:ALL
            lock_passwd: false
        chpasswd:
          expire: false
          users:
          - name: nutanix
            password: nutanix/4u
            type: text
        runcmd:
        - '[ ! -f "/etc/yum.repos.d/nutanix_rocky9.repo" ] || mv -f /etc/yum.repos.d/nutanix_rocky9.repo /etc/yum.repos.d/nutanix_rocky9.repo.disabled'
        - dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
        - dnf -y install git docker-ce docker-ce-cli containerd.io
        - systemctl --now enable docker
        - usermod -aG docker nutanix
        - 'curl -Lo /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
        - chmod +x /usr/local/bin/kubectl
        - 'curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash'
        - 'su - nutanix -c "curl -fsSL http://10.42.194.11/workshop_staging/tradeshows/experimental/nkp-bootcamp/install-tools.sh | bash"'
        - eject
        ```
        
          
        
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
          
        
        Cloud-config explanation
        
        -   **2** Update the value with your user. Ex: user**01**
        -   **3-16** Create a new user `nutanix` and enable password support. It is standard for cloud images not to include a password out-of-the-box
        -   **15** The set default password is `nutanix/4u`. In production, you should use SSH public key and not password-based authentication
        -   **19-22** Add the Docker package repository, install Docker Engine and its CLI, and install containerd
        -   **23-24** Install the kubectl CLI and make it executable
        -   **25** Install the Helm CLI
        -   **26** Only required for the bootcamp. This installs a web-version of VS Code
        
        ![Cloud-init](/cloudnative/assets/3-create-vm-management.de2d3a3b.png)
        
8.  Review the configuration and click **Create VM**
    
9.  **Power On** your VM