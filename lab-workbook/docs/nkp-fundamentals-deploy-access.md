# NKP Advanced Hands-on Lab

# [#](#accessing-the-shared-nkp-environment) Accessing the shared NKP environment

Starting from this lab you'll be using the staged multi-cluster setup mentioned at the beginning of the bootcamp. In order to interact with the environment, you must retrieve an access token first to use with the CLIs in your terminal.

Please do not perform destructive actions

You are sharing this environment with other users.

1.  Access the shared NKP management console on a new tab. If you are unsure of the connection details, ask your instructor.
    
    Ex: _https://`#.#.#`.16/dkp/kommander/dashboard_
    
    accept the self-signed certificate.
    
    ![Dashboard URL](/cloudnative/assets/dashboard-url.0d38f8c0.png)
    
2.  Choose `log in with ntnxlab.local`, and use your credentials
    
    You don't need to add the domain in your Username
    
    ![AD authentication](/cloudnative/assets/dashboard-ldap-auth.e90e9704.gif)
    
3.  Click your user at the top right, followed by **Generate Token**
    
    ![Generate token](/cloudnative/assets/dashboard-token.d17bb54b.png)
    
4.  Choose the **Main** NKP cluster (management) because this is where you'll be deploying the example application
    
    ![Token main cluster](/cloudnative/assets/token-main-cluster.70ced8b8.png)
    
5.  Re-authenticate with your adminuser`##`
    
6.  Back in VS Code open a new terminal, use the `+` icon next to the _TERMINAL_ bar at the bottom
    
    ![New terminal](/cloudnative/assets/new-terminal.d7f452d7.png)
    
7.  Follow the `Linux` steps pasting the commands in the new terminal
    
    Ignore the install and set up kubectl. It was installed as part of the Admin VM creation.
    
    ![Paste token](/cloudnative/assets/token-steps.66812230.png)
    
8.  Once you have finished with the **the 5 code blocks**, confirm you can interact with the cluster
    
    -   command
    -   output (example)
    
    ```
    kubectl get nodes
    ```
    
    ```
    NAME                         STATUS   ROLES           AGE    VERSION
    nkp-brfn6-68ppw              Ready    control-plane   12h   v1.34.1
    nkp-brfn6-rsfcq              Ready    control-plane   12h   v1.34.1
    nkp-brfn6-xkclb              Ready    control-plane   12h   v1.34.1
    nkp-md-0-rbns5-fvdf8-7qfjq   Ready    <none>          12h   v1.34.1
    nkp-md-0-rbns5-fvdf8-l22vp   Ready    <none>          12h   v1.34.1
    nkp-md-0-rbns5-fvdf8-qzbdn   Ready    <none>          12h   v1.34.1
    nkp-md-0-rbns5-fvdf8-vkkpn   Ready    <none>          12h   v1.34.1
    ```
    
    WARNING
    
    If the previous command throws an error or authentication request, ensure you applied **all** the **5** code blocks from the step before
    

Now you are set to operate with the shared NKP management cluster.