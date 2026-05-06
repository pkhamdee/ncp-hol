# Nutanix Unified Storage

# [#](#file-analytics-anomaly-rules) File Analytics: Anomaly Rules

## [#](#define-anomaly-rules) Define Anomaly Rules

1.  Within the menu, select **Define Anomaly Rules**.
    
2.  Click **\> Define Anomaly Rules**. Fill in the following fields and click .
    
    -   **Events:** Delete
    -   **Minimum Operation %:** `1`
    -   **Minimum Operation Count:** `10`
    -   **User:** All Users
    -   **Type:** Hourly
    -   **Interval:** `1`
3.  Click **\> Configure new anomaly**. Fill in the following fields and click **\> Save**.
    
    -   **Events**: Create
    -   **Minimum Operation %**: `1`
    -   **Minimum Operation Count**: `10`
    -   **User**: All Users
    -   **Type**: Hourly
    -   **Interval**: `1`
    
    ![](/unified-storage/assets/1.fe29b12d.png)
    

## [#](#load-sample-data) Load Sample Data

1.  Return to your **`User##`\-WinTools** Remote Desktop session.
    
2.  Within the _`User##`\-Marketing_ folder, click on **Sample Data**. Right-click on **Sample Data** and choose **Copy**. Right-click on any blank space below the files/folder, and choose **Paste**. You will now have an additional folder named **Sample Data - Copy**. 
    
3.  Right-click on the **Sample Data** folder, and select **Delete**.
    
    By default, the Anomaly engine runs every 30 minutes. While this setting is configurable from the File Analytics VM, modifying this variable is outside the scope of this lab. Therefore, it may take up to 30 minutes the _Anomalies Alerts_ section will update with the "anomalous" activity you just performed. If you'd prefer not to wait, we've provided an example of what you would see.
    
    ![](/unified-storage/assets/3.155f4f1c.png)
    

## [#](#cause-error-condition) Cause Error Condition

1.  Return to your **`User##`\-WinTools** Remote Desktop session.
    
2.  Within _`User##`\-Marketing_ folder, create a new directory named `RO` (Read Only).
    
3.  Within the **RO** directory, Create a new text file containing the text `hello world`. Save the file as **myfile.txt**.
    
4.  Go to the _Properties_ of the _RO_ folder. Select the **Security** tab, and click **Advanced**.
    
5.  Click **Disable inheritance** and select **Convert inherited permissions into explicit permissiong on this object.**.
    
6.  Click **Add**. Click **Select a principal**. Within _Enter the object name to select_, enter `everyone` and click **OK**. The default rights are sufficient for our use. Click **OK > OK > OK**.
    
7.  Right-click on the **Powershell** icon in the taskbar. Hold down **Shift** and right-click on the **Windows Powershell** list item, and choose **Run as a different user**.
    
8.  Enter the following:
    
    -   **User name**: `devuser##`
    -   **Password**: `nutanix/4u`
9.  Execute the `cd \\BootcampFS.ntnxlab.local\User##-Marketing\RO` command to change directory into the **RO** directory within the **`User##`\-Marketing** share.
    
10.  Execute the `more myfile.txt` command. As this command attempts to read and display the contents of the file, it will succeed since we have the _Read_ permission on this folder.
    
11.  Execute the `rm myfile.txt` command. As this command attempts to delete the file, it will fail since this user doesn't not have the _Delete_ permission on this folder.
    
    ![](/unified-storage/assets/6.2942fbcf.png)
    
12.  Close _Powershell_, and return to your _Files Analytics_ tab.
    
13.  After a few moments, your activity will be displayed within the _Top 5 Active Users_ section. Click on **`devuser##`**. The _Audit Details_ for _`devuser##`_ will open. Observe your activity within the _Total Result(s)_ section.
    
    ![](/unified-storage/assets/7.c1d53c59.png)![](/unified-storage/assets/8.9db6f1ae.png)
    
14.  Once you are through, click the in the top right corner.