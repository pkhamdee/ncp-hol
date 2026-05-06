# Nutanix Database Service (NDB) Lab

# [#](#database-provisioning-and-patching) Database Provisioning and Patching

Traditional database VM deployment resembles the diagram below. The process generally starts with an IT ticket for a database (from Dev, Test, QA, Analytics, etc.). Next, teams must deploy the required storage resources and VM(s). Once the infrastructure is ready, a Database Administrator (DBA) must provision and configure the database software. Once provisioned, best practices, including data protection and backup policies, must be applied. Finally, the database can be handed over to the end user. That's a lot of handoffs and the potential for friction.

![](/ndb/assets/n00.4edf9d4c.png)

Provisioning and protecting databases require significantly less time and effort using NDB. While performing the steps in the labs, consider how you would perform these tasks in your existing environment today and compare them.

## [#](#provision-postgresql-database) Provision PostgreSQL Database

As a Database administrator (DBA), you will need to create new database servers. NDB makes that process painless with its customized predefined profiles. Let's walk through provisioning a PostgreSQL database. Do not worry if you are unfamiliar with PostgreSQL; NDB simplifies this process. These steps taken during this provisioning workflow are similar for all of the databases supported by NDB.

1.  Within **NDB**, select **\> Databases** from the menu, and then **Sources** from the top menu.
    
2.  Click **Provision > PostgreSQL > Instance**.
    
3.  Fill out the following fields on the Database Server VM page, and click **Next**.
    
    -   **Database Server VM** - Create New Server
    -   **Database Server VM Name** - `User##-mypsql`
    -   **Software Profile** - POSTGRES\_15.6\_ROCKY\_LINUX\_8\_OOB
    -   **Compute Profile** - CUSTOM\_EXTRA\_SMALL
    -   **Network Profile** - DEFAULT\_OOB\_POSTGRESQL\_NETWORK
    -   **SSH Public Key for Node Access** - Select **Text**, then copy/paste the below into the text field.
    
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoQRdEfm8ZJNGlYLQ2iw08eVk/Wyj0zl3M5KyqKmBTpUaS1uxj0K05HMHaUNP+AeJ63Qa2hI1RJHBJOnV7Dx28/yN7ymQpvO1jWejv/AT/yasC9ayiIT1rCrpHvEDXH9ee0NZ3Dtv91R+8kDEQaUfJLYa5X97+jPMVFC7fWK5PqZRzx+N0bh1izSf8PW0snk3t13DYovHFtlTpzVaYRec/XfgHF9j0032vQDK3svfQqCVzT02NXeEyksLbRfGJwl3UsA1ujQdPgalil0RyyWzCMIabVofz+Czq4zFDFjX+ZPQKZr94/h/6RMBRyWFY5CsUVvw8f+Rq6kW+VTYMvvkv
    ```
    
    ![](/ndb/assets/n01a.39cf26d5.png)![](/ndb/assets/n01b.0312fc1c.png)
    
    Note
    
    The above SSH public key is provided as an example and is configured as an authorized key for the operating system provisioned by NDB. In a non-lab setting you would create your own SSH private/public key pair, and provide the public key during this step.
    
4.  Fill out the following fields within the _Instance_ page, then click **Next**.
    
    -   **PostgreSQL Instance Name** - `User##-LabDB`
    -   **Database Parameter Profile** - DEFAULT\_POSTGRES\_PARAMS
    -   **Listener Port** - 5432 (default)
    -   **Size (GiB)** - 200 (default)
    -   **Name of Initial Database** - `User##initialdb`
    -   **Database user password** - `nutanix/4u`
    
    ![](/ndb/assets/n04.3be8877f.png)
    
    Note
    
    NDB also offers to ability to run scripts or commands both before and after database creation. These can be used to further customize an environment based on specific enterprise needs.
    
5.  Fill out the following fields within the _Time Machine_ page, then click **Next**.
    
    -   **Name** - `User##`\-LabDB\_TM (default)
    -   **SLA** - DEFAULT\_OOB\_GOLD\_SLA
    -   **Schedule** - (default)
    
    ![](/ndb/assets/4f2.612bbe7a.png)
    
    Note
    
    -   The Time Machine section sets the time you want full snapshots of the database to be taken, how to be taken in a day, and how often you want your log backups to run.
    -   NDB time machines capture and maintain snapshots and transaction logs of your source databases as defined in the Data Access Management policies (DAM policies) and SLA schedules.
    
6.  Fill out the following fields within theds within the **Additional Configurations** page, then click **Provision**.
    
    -   Under **Opt in/out for repeated automated scheduling**
        -   Check **Operating System Pathching**
        -   Check **Database Patching**
    -   **Maintenance Windows** - `user##-maintenance-policy`
    
    ![](/ndb/assets/9.60703453.png)
    
    Note
    
    -   The option to choose a maintenance policy during provisioning is another way the NDB helps keep your database VMs up to date and secure.
    -   PostgreSQL and MongoDB support database encryption. You can learn more in the [NDB documentationopen in new window](https://portal.nutanix.com/page/documents/list?type=software&filterKey=software&filterVal=Nutanix%20Database%20Service) on the Nutanix Portal.
    
7.  Select **\> Operations** from the drop-down menu to monitor the provisioning. This process will take <20 minutes.
    
    Note
    
    All operations within NDB have unique IDs are fully visible for logging/auditing.
    

While your database vm in provisioning, lets move on to the [On Demand OS Patching](#on-demand-os-patching) section.

## [#](#on-demand-os-patching) On Demand OS Patching

As a DBA, you may be responsible for patching the operating system when you manage multiple database servers. You were just alerted about a critical security flaw in your database VMs. The patch has been pushed to the repository, so you must update the affected VMS database quickly. Let's get started

1.  Select **\> Database Server VMs** then select **list** From the top menu.
    
    ![](/ndb/assets/20.4255e03b.png)
    
2.  Click on your `User##`\-postgres from the list of VMs, this shold take you to the database servers details page as show below.
    
    ![](/ndb/assets/21.d8923698.png)
    
3.  Select **\>Actions > Patch OS Now**
    
4.  On the **Patch OS Now** screen do the following then click **Update**.
    
    -   Expand **Custom Commands > Operating System Patching Command** : enter `sudo dnf update -y --security`
    -   Under **Reboot Needed** : Select Yes
    
    ![](/ndb/assets/22.e7d1968a.png)
    
    Note
    
    -   We choose to reboot yes in this case, but you are given the option to reboot later if needed-
    -   Since this was a security flaw we chose to --security option for RHEL based VMs.
    -   You can substitute yum in RHEL 9 based and earlier distribution.
    -   For Ubuntu (Debian)based distributions, you can set up the unattended-upgrade package to perform security updates only. For more information, see your distribution's documentation. That can be run by putting sudo apt-get `package_name` in the custom command
    
5.  Click on the green bar to check the progress of the patching operation.
    
    ![](/ndb/assets/23.f98533e1.png)
    

That's how quickly you can patch a day-zero vulnerability on a database VM operating system with NDB. You would need to repeat this on any of the other affected VMs in your environment. NDB will use the patch repository that is set up on the database VM to pull patches from. However, that only patches the database operating system.

NDB can patch your database servers in bulk using the Maintenance Window we set up at the beginning of the lab. While your database OS is patching, let's see how to set up scheduled bulk patching.

## [#](#scheduled-database-patching) Scheduled Database Patching

1.  Select **\>Database Server VMs**, then click **List** on the top menu.
    
    ![](/ndb/assets/24.be216d06.png)
    
2.  Select your `user##-postgres` VM from the list.
    
    ![](/ndb/assets/25.99dd16e3.png)
    
3.  Select the Actions Dropdown then Maintenance
    
    ![](/ndb/assets/26.e4d8b16b.png)
    
4.  Select the following options:
    
    -   **Operating System Patching** - Check
    -   **Database Patching** - Check
    -   **Maintenance Window** - Select `user##-maintenance-policy`![](/ndb/assets/27.d1bce96a.png)
    
    You can choose to schedule OS Patching or Database patching only or both. You can also select multiple VMs at a time to associate with a maintenance window policy. Once you have made the selections, click **Associate**
    
    Note
    
    -   PostgreSQL, MongoDB, and Oracle are currently supported for bulk patching with Maintenance Window Policies for database software updates and operating systems.
    -   Microsoft SQL server is currently supported for only database software updates using the Maintenance Window Policy feature.
    

Now that we have setup patching for our database VM, let’s move on to [clone creation and management](/ndb/clone_postgresql/clone_postgresql.html).