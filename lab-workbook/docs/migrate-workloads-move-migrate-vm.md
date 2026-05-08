# Migrate VM

Once the Migration plan is started, it will first be validated.

1.  Verify Plan Validation in the Status section. ![](/images/vm-migration-plan10.94060ba9.png)
    
2.  Once validation succeeds, the status changes to **In Progress**. Click `In Progress` to monitor the status.
    
    ![](/images/vm-migration-plan11.26e087a1.png)
    
3.  You can monitor the **Migration Status** and **Details** on this page. This should take about 5-10 minutes. A perfect time to get a cup of ☕.
    
    For curious minds, during this step, Move is preparing the source VM for transfer by doing tasks like installing Nutanix VirtIO drivers and doing data seeding on the target cluster. For a detailed list of tasks done refer to the [Move Guide](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Move-v5_5:top-create-migration-plan-t.html)
    
    ![](/images/vm-migration-plan12.d3eef821.png)
    
    ![](/images/vm-migration-plan13.3f1bdfad.png)
    
4.  Once the data seeding is complete, the status becomes `Ready To Cutover`. It also gives an estimate for cutover time so you can plan the actual cutover.
    
    ![](/images/vm-migration-plan14.ac1e3c91.png)
    
    If you are on the main page of Move, a blue dot means it is ready for cutover.
    
    ![](/images/vm-migration-plan15.f2ce5b29.png)
    
5.  To perform the cutover, Select the VM and Click `Cutover`. Click `Continue` to confirm.
    
    ![](/images/vm-migration-plan16b.b0764761.png)
    
    ![](/images/vm-migration-plan16c.3e2a1590.png)
    
    Press `Continue` on the splash screen.
    
6.  You can monitor the cutover in **Migration Status** and **Details**.
    
    ![](/images/vm-migration-plan17.3efe4d4b.png)
    
7.  Once the cutover is complete, **Status** will say Completed and **Details** will provide a link to View Target VM.
    
    **DO NOT CLICK ON View Target VM**. This will take you to Prism Element, but we want to log back into Prism Central instead.
    
    ![](/images/vm-migration-plan18.2649624b.png)
    
8.  Login to Prism Central
    
    -   **username** - `<PC login> will be adminuser##@ntnxlab.local` or `adminuser##`
    -   **password** - `<PC password>` from Connection Details
9.  Navigate to **Compute & Storage** > **VMs**
    
    ![](/images/vm-migration1.dcec209e.png)
    
10.  Look for your migrated VM in the main page or you can search in the search bar. Make a note of your VM's IP address.
    
    ![](/images/vm-migration-plan19b.032f31de.png)
    
11.  You will be using the IP address of your migrated VM to connect to a code-server via the browser in the Parallels VDI environment.
    
    -   **link** - `http://vmIP:8001`
    
    ![](/images/vm-migration-code-servera.f19336eb.png)
    
12.  You will then be able to log into the code-server with the following:
    
    -   **password** - `adminuser01`
    
    !!! note    
        All users will use `adminuser01` as the password regardless of what other passwords you've previously used.
    
    You will then be able to access the terminal.
    
    ![](/images/vm-migration-code-serverb.3d81a448.png)
    
13.  In the console window, type the `ls` command and verify the file is present.
    
    ![](/images/vm-migration-plan21b.01199a6e.png)
    

🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉

CONGRATULATIONS!! You just completed a seamless move of your VM from AHV to AHV in a few clicks.

Next, if you have time, let's perform another migration with Move and explore several of the advanced functions that Move offers. Your instructor can help you determine which steps to take next.