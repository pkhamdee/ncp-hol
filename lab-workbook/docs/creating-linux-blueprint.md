# Nutanix Cloud Platform - Pt 2

# [#](#creating-a-single-vm-blueprint-linux) Creating a Single VM Blueprint (Linux)

A Blueprint is the framework for every application or piece of infrastructure that you model by using Nutanix Self-Service. While complex, multi-tiered applications utilize the Multi VM/Pod Blueprint, the streamlined interface of the Single VM Blueprint is conducive for IaaS use cases. You can model each type of infrastructure your company utilizes (for instance Windows, CentOS, or Ubuntu) in a Single VM Blueprint, and end users can repeatedly launch the Blueprint to create infrastructure on demand.

The resulting infrastructure (which is referred to as an application), can then be managed throughout its entire lifecycle within Self-Service, including managing Nutanix Guest Tools (NGT), modifying resources, snapshots, and clones.

## [#](#create-blueprint) Create Blueprint

1.  Login to Prism Central using `adminuser##` and the PC password from the Connection Details page.
    
2.  Navigate to the App Switcher section in the top left of Prism Central. Click **Self Service** in the App Switcher.
    
3.  Click **Blueprints** in the left toolbar.
    
4.  Click **Create Blueprint** > **Single VM Blueprint**.
    
5.  Enter the below information:
    
    -   Name - **User`##`\-CentOS-IaaS** where `##` is your assigned number.
    -   Project - Initials-FiestaProject
    -   Environment - Initials-Environment
    
    Note:
    
    The Project and Environment should be the same which you had created as part of the Projects exercise
    
6.  Click **VM Details**.
    
7.  Fill out the following fields on the VM Details page:
    
    -   Name - **User`##`\_VM** where `##` is your assigned number
    -   Operating System - Select Linux from the drop-down.
8.  Click **VM Configuration**.
    
9.  Click **Clone from environment**. Since you've previously specified the details for your Linux VM within your Project, there's no need to enter that same information again.
    
    ![](/images/selfservice11.77b42afc.png)
    
    Note:
    
    Self-Service macros are part of a templating language for Self-Service scripts. These are evaluated by Self-Service execution engine before the script is run.
    
10.  In General Configuration - Enter **vm-@@{calm\_array\_index}@@-@@{calm\_time}@@** within the VM Name field.
    
11.  Check the **Guest Customization** box, and paste in the following script. Guest Customization - Guest customization allows for the modification of certain settings at boot. Linux uses cloud-init, while Windows uses sysprep.
    

```
#cloud-config
users:
  name: centos
  sudo: ['ALL=(ALL) NOPASSWD:ALL']
chpasswd:
  list: |
    centos:@@{CENTOS.secret}@@
  expire: False
ssh_pwauth:   true
```

12.  Click **Advanced Options**.
    
13.  Click **Add/Edit Credentials**. This is required to make any changes to this entire section.
    
14.  Click **Add Credential**, fill out the following fields.
    

-   Name CENTOS
-   Username nutanix
-   Password nutanix/4u
-   Click **Done**

15.  **(Optional)** Scroll down to the Update Configs section, and click **Add Config**.
    
16.  Enter **User`##`UpdateConfig** where `##` is your assigned number within the Name the update configuration field,
    
17.  Click **Update** on the right side of Memory (GiB) row, fill out the following fields,
    
    -   Memory (GiB) - Increase
    -   Update - 1
    -   Enable the Editable switch
    -   Max Value - 6
    -   Click **Done**
    
    ![](/images/selfservice3.2e702ef6.png)
    
18.  **(Optional)** Click on **Add Snapshot/Restore Config** within the Snapshot/Restore section
    
19.  Fill out the following field, and click Save. This is required to allow the user to snapshot the application.
    
    -   Snapshot/Restore action suffix - **User`##`** where `##` is your assigned number.
    -   Click the 1 button.
    
    ![](/images/selfservice4.906e7d8e.png)
    
20.  Click **Save**
    

Great. You have successfully created your first Single VM Blueprint!!! 🎉

## [#](#defining-variables) Defining Variables

Variables allow extensibility of Blueprints. This means a single Blueprint can be used for multiple purposes and environments depending on the configuration of its variables. Variables can either be static values saved as part of the Blueprint, or they can be specified at runtime (when the Blueprint is launched), as they will in this case.

In a Single VM Blueprint, variables can be accessed by clicking the App variables button near the top. By default, variables are stored as a String, however additional Data Types (Integer, Multi-line String, Date, Time, and Date Time) are all possible. Any of these data types can be optionally set as Secret, which will mask its value and is ideal for variables such as passwords. There are also more advanced Input Types (versus the default Simple), however these are outside the scope of this lab.

Variables can be used in scripts executed against objects using the `@@{variable_name}@@` construct (called a macro). Self-Service will expand and replace the variable with the appropriate value before sending to the VM.

1.  Click Blueprints --> **User`##`\-CentOS-IaaS** where `##` is your assigned number.
    
2.  Click the App variables button along the top pane to bring up the variables menu.
    
    ![](/images/selfservice5.769e6a71.png)
    
3.  In the pop-up that appears, you should see a note stating you currently do not have any variables. Click the **Add Variable** button, and fill out the following fields.
    
    -   Within the left column, click the **running person** icon to mark this as a runtime variable.
        
        ![](/images/selfservice6.b8295994.png)
        
    -   In the main pane, set the variable Name as **vm\_name\_prefix**.
        
    -   Click the **Show Additional Options** link at the bottom.
        
    -   Check the **Mark this variable mandatory** checkbox. This ensures a value is input, as this variable contributes to the VM name.
        
    
    ![](/images/selfservice7.57dc956e.png)
    
4.  Click **Done**
    
5.  Click **Save** to save the changes made to the blueprint
    

## [#](#marketplace) Marketplace

Now that we know we have a known-good Blueprint, let's publish it to the Marketplace.

## [#](#publishing-the-blueprint) Publishing the Blueprint

1.  Click Blueprints --> **User`##`\-CentOS-IaaS** where `##` is your assigned number.
    
2.  Click the **Publish** button, and enter the following:
    
    -   Name - **User`##`\-CentOS-IaaS** where `##` is your assigned number.
    -   Enable Publish with Secrets
    -   Initial Version - 1.0.0
    -   Description - Standard CentOS VM Deployment
    -   Click **Submit for Approval**

## [#](#approving-blueprints) Approving Blueprints

Once the blueprint is submitted, it also needs to be approved. This process makes more sense when you think about a larger team of blueprint creators and then a smaller group who promotes ready blueprints for consumption. Approved blueprints go to production for others to use.

1.  Click **Marketplace Manager**
    
2.  You will see the list of Marketplace Blueprints, and their versions listed.
    
3.  Select **Approval Pending** at the top of the page.
    
4.  Click on the **User`##`\-CentOS-IaaS** Blueprint where ## is your assigned number
    
5.  Select Initials-FiestaProject from the drop-down of the Projects Shared With field.
    
6.  Click the **Approve** button
    
    ![](/images/selfservice8.d37adcd2.png)
    
7.  Select **Approved** at the top of the page.
    
8.  Click on the **User`##`\-CentOS-IaaS** Blueprint where `##` is your assigned number
    
9.  Click the **Publish** button.
    
10.  Click the **Deploy** button
    
11.  Give your app a name and for the vm\_name\_prefix use **User`##`\_LinPrefix**
    
12.  Click the **Deploy** button
    

Note:

You have successfully published your application to the Marketplace. You can view it by going to **Admin Center** > **Marketplace**

![](/images/selfservice9.87e352b0.png)