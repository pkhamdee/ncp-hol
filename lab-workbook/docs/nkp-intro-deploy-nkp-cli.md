# NKP Advanced Hands-on Lab

# [#](#install-nkp-cli) Install NKP CLI

1.  In your VS Code terminal, check Docker is running
    
    -   command
    -   output
    
    ```
    docker ps
    ```
    
    ```
    CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
    ```
    
2.  Execute the following command. It is a script wrapper for installing the CLI (URL is available in the step after)
    
    -   command
    -   output
    
    ```
    curl -fsSL https://raw.githubusercontent.com/nutanixdev/nkp-quickstart/main/get-nkp-cli | bash
    ```
    
    ```
    Enter 'NKP for Linux' download link:
    ```
    
    Note
    
    The script is not officially supported. It just captures the steps explained in the [official documentationopen in new window](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Kubernetes-Platform-v2_16:top-download-nkp-t.html) for installing the NKP CLI.
    
3.  When prompted, enter the following URL
    
    ```
    http://10.42.194.11/workshop_staging/tradeshows/software/nutanix/kubernetes/nkp/nkp_v2.17.0_linux_amd64.tar.gz
    ```
    
    (Optional) Where to find the NKP CLI when not in a bootcamp
    
    You can find the NKP CLI download link on the [Nutanix portalopen in new window](https://portal.nutanix.com/page/downloads?product=nkp).
    
    ![NKP CLI Portal](/cloudnative/assets/nkp_cli_portal_link.9095dade.png)
    
4.  Confirm the NKP CLI was installed
    
    -   command
    -   output
    
    ```
    nkp version
    ```
    
    ```
    catalog: v0.8.1
    diagnose: v0.12.0
    imagebuilder: v2.17.0
    kommander: v2.17.0
    konvoy: v2.17.0
    konvoybundlepusher: v2.17.0
    mindthegap: v1.24.0
    nkp: v2.17.0
    ```