# Untitled Page

# [#](#configuring-nutanix-files) Configuring Nutanix Files

In this section, you will create a Nutanix Files share on our automatically provisioned cluster. This share is the destination for the source share we are migrating.

## [#](#creating-a-nutanix-files-share) Creating A Nutanix Files Share

We have already created a 1-node Files instance called `nextfiles` running in the lab for this exercise. Users will log onto Prism Central, access Files from there, and create a unique share. This share will also serve as the destination share for data migration in the next chapter.

1.  Connect to Prism Central from within your VDI session. Ensure the Login screen says **Login with your Company ID**
    
    -   **username** - `<PC username> adminuser##@ntnxlab.local` or `adminuser##`
    -   **password** - `<PC password provided>` from Connection Details

![](/images/pc-login.0df391a6.png)

1.  Navigate to the App Switcher section in the top left of Prism Central.
    
    Note
    
    You can use the App Switcher to quickly navigate between sections in Prism Central.
    
    Click `Files` in the App Switcher under Unified Storage.
    
    ![](/images/open-files.12edd40b.png)
    
    A file server called **nextfiles** has already been deployed for your use. Click on the name `nextfiles` to access it.
    
    ![](/images/nextfiles.edaf5e80.png)
    
2.  Click on `Shares & Exports` from the top menu.
    
    ![](/images/open-shares.8317afbc.png)
    
3.  Click `+New Share or Export`.
    
    ![](/images/new-share.9eac7203.png)
    
4.  Enter `user##` within the **Name** field where `##` is your assigned `User #` from Connection Details. For the rest, leave the default values. For your information, these are the field definitions. Click `Next`
    

-   Description: User-friendly description of share.
    
-   Share Path: Only for share/export on an existing path.
    
-   Max Size: If you want to restrict the size of share/export to a particular size.
    
-   Primary Protocol Access: Select `SMB (Ideal for Windows Clients)`, since the source share is Windows.
    
    ![](/images/create-file-share1.f799c6c7.png)
    

5.  On the General Settings page, keep the default values. For your information, these are the field definitions. Click `Next`
    
    -   Enable Self-Service Restore: Allows users to restore files on File Server.
    -   Enable Compression: Enable Compression on share to provide space savings.
    -   Blocked File Types: Restrict file types that can be stored on the share/export. ![](/images/create-file-share2.11134c2f.png)
6.  On the Protocol Settings and Permissions page, keep the default values. For your information, these are the field definitions. Click `Next`
    
    -   Enable Access Based Enumeration (ABE): Restrict access to files and folders to only the ones that users have access to.
    -   Encrypt SMB3 Messages: Encrypt SMB messages between Clients and shares.
    -   SMB Continuous Availability: Allows File share access during planned or unplanned outages.
    -   Share Permissions: Setup access permissions for Users and Groups for the share
    
    Note
    
    If we were working with a 3-node or larger file server, there would also be the option to designate the share as **Distributed**. This would distribute top-level directories across all File Server VMs (FSVM), load balance connections and data across the Files cluster.
    
    In this case, we use a standard share, where the file server is serviced from a single FSVM.
    
    ![](/images/create-file-share3.95f8021f.png)
    
7.  On the Summary page, Confirm the settings and click `Create`. The share will appear in a few moments, and is available for storing files.
    
    ![](/images/create-file-share4.74e50973.png)![](/images/create-file-share5.fbf0248d.png)
    

## [#](#create-rest-api-user) Create REST API user

To perform share migration with Move, Move requires a REST API user to be created on Nutanix Files. Let's do that.

1.  Click `Configuration > Manage Roles` from the Nutanix Files top menu

![](/images/new-api-user.7a9f9220.png)

2.  In REST API access users, Click `+New User`
    
    ![](/images/new-api-user2.f39cdf32.png)
    
3.  Create a New user with the following credentials and click the check mark. Remember this username and password for the next section. You're free to choose a different password, but you'll need this later.
    

-   **username** : `adminuser##`
-   **password** : `dotNext25!`

Note

Remember to click the check mark.

![](/images/new-api-user3.8f91f596.png)

4.  From the App Switcher, you can go back to Infrastructure section.

Now that we have the environment set up and configured, in the next section you will learn how to migrate file shares.