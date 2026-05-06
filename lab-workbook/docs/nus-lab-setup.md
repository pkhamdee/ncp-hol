# Nutanix Unified Storage

# [#](#lab-setup) Lab Setup

1.  Open Chrome, enter your Prism Element IP address, and log in using the provided credentials.
    
2.  Within the drop-down, select **VM** and click on **Table**.
    
3.  Locate both your **`User##`\-WinTools** and **`User##`\-LinuxTools** VMs. Note their IP addresses, using the **IP Addresses** column.
    

![](/unified-storage/assets/ip_addresses.63dcc6c9.png)

4.  Connect to your **`User##`\-WinTools** VM via Remote Desktop using the following credentials:
    
    -   Username **administrator@ntnxlab.local**
    -   Password: **cluster password**

More detailed instructions can be found [here](/unified-storage/appendix/remote_desktop.html).

5.  Within your **`User##`\-WinTools** VM, download the following sample data file by copying and pasting this URL into Chrome.

```
http://10.42.194.11/hol/unified-storage/SampleData_Small.zip
```