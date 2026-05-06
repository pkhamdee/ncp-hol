# Nutanix Unified Storage

# [#](#file-analytics-file-system-scan) File Analytics: File System Scan

1.  Return to your **Prism Element** browser tab.
    
2.  Within the drop-down menu, select **File Server**.
    
3.  Click on **BootcampFS**, and then click **File Analytics**.
    
    ![](/unified-storage/assets/1.08142b61.png)
    
4.  Log in using your Prism Element credentials.
    
5.  Click **\> Scan File System**.
    
    ![](/unified-storage/assets/2.fa80fa65.png)
    
6.  Check the **`User##`\-Marketing** entry, and click **Scan** to perform an initial scan of the existing shares, which will take <1 minute. Click once the scan status is **Completed**. You will now see the dashboard panels update.
    
    ![](/unified-storage/assets/3.d052733f.png)
    
7.  Return to your **`User##`\-WinTools** Remote Desktop session.
    
8.  Open one of the Word files within **`User##`\-Marketing > Sample Data > Documents**.
    
    Note
    
    If you are prompted to complete the wizard for OpenOffice, click **Next > Finish**.
    
9.  Return to the Files Analytics browser tab. Refresh the _Dashboard_ page. The **Top 5 Active Users** and **Top 5 Accessed Files** panels have now been updated to report your activity.
    
    ![](/unified-storage/assets/4.3eed217a.png)
    
10.  Within **Top 5 Active Users**, click on the username (ex., adminuser20), which will take you to the audit trail of the user.
    
11.  You can also click on the **\> Audit Trails** menu and search for activity. Wildcards can be used within search (ex. **`*.doc*`**).
    
    ![](/unified-storage/assets/5.1251bd09.png)
    
12.  Click once you are finished viewing the activity.
    

## [#](#takeaways) Takeaways

File Analytics helps you better understand how your organization utilizes data to help you meet your data auditing, data access minimization, and compliance requirements.