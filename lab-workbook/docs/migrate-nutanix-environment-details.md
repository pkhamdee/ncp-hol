# Untitled Page

# [#](#environment-details) Environment Details

Nutanix Bootcamps are intended to run within the Nutanix Hosted POC (HPOC) environment. Your cluster contains all the necessary images, networks, and VMs to complete the exercises.

Danger

Do not perform any upgrades to the environment, including but not limited to Prism Element (PE), Prism Central (PC), Acropolis Operating System (AOS), Nutanix Cluster Check (NCC), Foundation, any hardware-specific updates (ex. firmware), and any software within any remote sessions (ex. Graylog, Linux packages, PuTTY, Sublime Text).

Doing so will negatively impact your lab experience and potentially any other attendees using this cluster.

## [#](#remote-connection) Remote Connection

1.  If you're a Nutanix employee, disconnect from the corporate VPN.
    
2.  Follow the instructions provided in the Hands-on Lab portal to get connected to the **Parallels** remote desktop using your seat number. Your instructor will have these details.
    
3.  Use Google Chrome Incognito mode to connect to the VDI environment.
    
4.  Be patient while connecting to the VDI session and note that resizing or changing the VDI resolution will result in reconnection, so is not recommended.
    

!!! warning
    Always use applications within the remote session. Otherwise, the version you use may look or operate differently, negatively impacting your ability to complete the lab in the allotted time. Additionally, this aids with handling downloaded files, as all files would be within the remote session.

## [#](#know-before-you-go) Know Before You Go

-   Never use the information within screenshots in your environment (ex., IP addresses.) Screenshots are shown for illustration purposes only.
    
-   Ignore any IP addresses that resemble **169.254.###.###**.
    
-   Throughout the lab, you will see the usage of **##**. Replace the **##** with your assigned number (ex., adminuser**01**).
    
    For example:
    
    -   Active Directory user - adminuser**01**@ntnxlab.local
    -   Move VM: User**01**\-Move
    -   Desktop VM: Desktop**01**
-   If instructed to:
    
    -   SSH (Secure Shell): Use PuTTY within your VDI desktop/session.
    -   Remote Desktop, Remote Desktop Protocol (RDP), or Remote Desktop Connection (RDC): **Remote Desktop Connection** via **Start Menu > Remote Desktop Connection**.
-   If you are presented with a security warning in Chrome, use one of the following methods to proceed.
    
    -   Method 1
        
        1.  Click **Advanced**. ![](/images/chrome_warning_method1-1.c637eb91.png)
        2.  Click on any blank section within the main browser window.
        3.  Type `thisisunsafe`. Note: The text you type will not be visible. ![](/images/chrome_warning_method1-2.bc602d1a.png)
    -   Method 2
        
        1.  Click **Advanced**![](/images/chrome_warning_method2-1.c9f6edc6.png)
        2.  Click **Proceed to _IP-ADDRESS_ (unsafe)**. ![](/images/chrome_warning_method2-2.fbe9a940.png)

## [#](#connect-to-prism-central-pc) Connect to Prism Central (PC)

1.  Make sure you are connected to your VDI
    
    Pro Tip
    
    Use **CTRL + V** to paste between your computer and the VDI (don't use the _command_ key in Mac)
    
2.  Login to Prism Central by opening the Prism Central link from Connection Details.
    
3.  Ensure the Login screen says **Login with your Company ID**.
    
4.  Replace `##` with your `User #`. For example, if your `User #` is 01, login as `adminuser01`. Note that seat numbers 01, 11, 21, 31, and so on will all map to `User #` 01.
    
    -   **username** - `<PC username> adminuser##@ntnxlab.local` or `adminuser##`
    -   **password** - `<PC password provided>` from Connection Details

![](/images/pc-login.0df391a6.png)

5.  Skip the tour.
    
    ![](/images/pc_skip_tour.edfe3379.png)