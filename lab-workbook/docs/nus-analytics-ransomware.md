# Nutanix Unified Storage

# [#](#file-analytics-ransomware) File Analytics: Ransomware

In this exercise, you will enable File Analytics ransomware protection and trigger a ransomware event.

1.  Navigate to **\> Ransomware**.
    
2.  Click **Enable Ransomware Protection > Enable**.
    
3.  Return to your **`User##`\-WinTools** Remote Desktop session.
    
4.  Open a PowerShell window by clicking the **PowerShell** icon on the taskbar.
    
5.  Execute the `new-item \\BootcampFS.ntnxlab.local\user##-marketing\ransomware.aaa -ItemType file` command. You will receive an access denied error message.
    
6.  Within **File Analytics** review the **Vulnerabilities** table to see the blocked operation. Note that you may need to refresh the **Ransomware** page.
    
    ![](/unified-storage/assets/ransomware_attempts.a5ec6763.png)
    
    Note
    
    You can also view the _permission denied_ event in the **Audit Trails**.
    

Nutanix Data Lens provides advanced analytics and ransomware protection above and beyond File Analytics, including over 5,000 ransomware signatures and detecting unknown variants based on access patterns.