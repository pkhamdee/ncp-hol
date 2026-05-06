# Nutanix Unified Storage

# [#](#files-file-blocking) Files: File Blocking

## [#](#selective-file-blocking) Selective File Blocking

In this exercise, you will configure Files to block specific file extensions for the file server and the Marketing share.

1.  Within your Nutanix Files browser tab, navigate to **Configuration > Blocked File Types**.
    
2.  Within the _Blocked File Types_ field, enter the comma-separated list of extensions `.flv, .mov`, and click **Save**.
    
    ![](/unified-storage/assets/1.44c3e313.png)
    
3.  In your Windows Tools VM, open a PowerShell window by clicking the **PowerShell icon** on the taskbar. Enter the following command. Ensure you modify `User##` to match yours.
    
    ```
    new-item \\BootcampFS.ntnxlab.local\`User##`-marketing\MyMovie.flv -ItemType file
    ```
    
    This image illustrates before and after the blocked file extensions are specified.
    
    ![](/unified-storage/assets/2.615ea45d.png)
    
4.  Return to your Files browser tab, and navigate to **Shares > `User##`\-Marketing**.
    
5.  Click **Update > Next**.
    
6.  Check **Blocked File Types** and enter `.none` as the extension.
    
    ![](/unified-storage/assets/3.5a338186.png)
    
7.  Click **Next > Update** to complete the update.
    
8.  Return to PowerShell, and execute the command from step 3 once again. The command will now complete successfully.
    
    ![](/unified-storage/assets/4.5daf96ad.png)
    
    Blocked file type settings specified at the share level will override settings specified at the server level.
    
9.  Click the at the top left corner once you are ready to proceed to the next section.