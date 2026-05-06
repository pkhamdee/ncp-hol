# Nutanix Database Service (NDB) Lab

# [#](#create-and-manage-clones) Create and Manage Clones

You can easily clone within NDB using the Time Machine. Database clones are helpful for development and testing purposes, allowing non-production environments to utilize production data without impacting production operations. NDB clones utilize Nutanix-native copy-on-write cloning technology, allowing for zero-byte database clones. This space efficiency can significantly lower storage costs for environments supporting large database clones by vastly reducing the required storage capacity.

## [#](#clone-the-fiesta-database) Clone the Fiesta Database

As a DBA, you have been tasked with creating a clone of the Production inventory database to be used by development. This will need to be refreshed weekly by 6:00 a.m. on today's date. Let's walk through the process of creating that clone.

1.  Within NDB, select **\> Data Protection > Time Machines** Time Machines from the drop-down menu, then select **List** from the top menu. Next, click the dot next to your `User##`\-fiestadb\_TM entry.
    
    ![](/ndb/assets/n01.76184e9b.png)
    
2.  Click **Actions > Create a Clone of the PostgreSQL Instance**.
    
3.  Within the _Time/Snapshot_ section, select **Snapshot** the pick an existing snapshot.
    
    ![](/ndb/assets/n02.ec5c4ccc.png)
    
    Note
    
    -   Without creating manual snapshots, NDB also offers the ability to clone a database based on point-in-time increments, including Continuous (every second), Daily, Weekly, Monthly, or Quarterly. The SLA controls availability.
    -   You may see red on the schedule graph if your database is still recovering from the previous exercise.
    
4.  Click **Next**.
    
5.  Within the _Database Server VM_ section, fill out the following fields and click **Next**.
    
    -   **Database Server VM** - Create New Server
    -   **Database Server VM Name** - `User##`\-postgres\_dev
    -   **Compute Profile** - CUSTOM\_EXTRA\_SMALL
    -   **Network Profile** - DEFAULT\_OOB\_POSTGRESQL\_NETWORK
    -   **SSH Public Key** - Select **Text**, and then copy/paste the below into the text field
    
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoQRdEfm8ZJNGlYLQ2iw08eVk/Wyj0zl3M5KyqKmBTpUaS1uxj0K05HMHaUNP+AeJ63Qa2hI1RJHBJOnV7Dx28/yN7ymQpvO1jWejv/AT/yasC9ayiIT1rCrpHvEDXH9ee0NZ3Dtv91R+8kDEQaUfJLYa5X97+jPMVFC7fWK5PqZRzx+N0bh1izSf8PW0snk3t13DYovHFtlTpzVaYRec/XfgHF9j0032vQDK3svfQqCVzT02NXeEyksLbRfGJwl3UsA1ujQdPgalil0RyyWzCMIabVofz+Czq4zFDFjX+ZPQKZr94/h/6RMBRyWFY5CsUVvw8f+Rq6kW+VTYMvvkv
    ```
    
    ![](/ndb/assets/n03.042e2942.png)
    
6.  Within the _Instance_ section, fill out the following fields
    
    -   **Name** - `User##`\-fiestadb\_dev
    -   **POSTGRES Password** - `nutanix/4u`
    -   **Database Parameter Profile** - DEFAULT\_POSTGRES\_PARAMS
    
    ![](/ndb/assets/n04.24ba1b38.png)
    
7.  Since it was requested that this clone be refreshed from production weekly starting today, check **Schedule Data Refresh** and fill out the following fields.
    
    -   **Refresh Every (days)** 7
    -   **Refresh Time** 00:06:00
    
    ![](/ndb/assets/n05.4d000795.png)
    
    Once the above fields are filled out, click **Clone**.
    
    Note
    
    -   If this database clone was only needed for a specific period of time, you could choose Removal Schedule and automatically set a date for the database clone to be removed.
    -   You can set or change the Schedule Data Refresh,\\ or Removal Schedule in the Time Machine of the database if it was not selected on clone creation.
    -   The Pre-Post Commands section can be used to run scripts that can be used to add or remove users and/or sanitize the database.
    
8.  You can check on the progress of your clone creation by clicking on the blue box that says **click here**.
    
    ![](/ndb/assets/n06.504c38b1.png)
    

Once you have verified that the Clone creation has started, we can proceed to the [Refresh Cloned Database](#refresh-cloned-database) section.

## [#](#refresh-cloned-database) Refresh Cloned Database

The ability to easily refresh a cloned database using the most up-to-date data from the source database improves development, testing, and other use cases by ensuring these individuals have access to the latest data. You have received a request to refresh a database from the source; let’s get that request completed.

1.  Select **\> Databases** then chose **Clones** from the top menu.
    
2.  Select your `User##-fiestadb_clone` and click **Refresh**.
    
    ![](/ndb/assets/n07.dac95e65.png)
    
3.  The latest available _Point in Time_ is selected by default. Click **Refresh**.
    
    TIP
    
    If a point in time snapshot is not available, select snapshot and pick a snapshot from the drop down. (This could be due to when the lab environment was created.)
    
    ![](/ndb/assets/n08.533f2e3b.png)
    
4.  Select **\> Operations** from the menu to monitor the _Refresh Clone_ process.
    

On the operations page, you should see the status of the clone-creation task from above and the refresh clone task. These tasks will be completed quickly as NDB uses the Nutanix Cloud Platforms' built-in snapshots, which are thin and space-efficient. This allows for quick creation and refreshing of clones.

## [#](#takeaways) Takeaways

NDB:

-   Supports one-click operations for registering, provisioning, cloning, and refreshing supported databases.
-   Enables the same type of simplicity and operating efficiency that you would expect from a public cloud provider while allowing DBAs to maintain control.
-   Automates complex database operations, slashing the time required by a DBA and the cost of managing databases with traditional technologies (Opex savings).
-   Empowers DBAs to standardize their database deployments across database engines while automatically incorporating each database's best practices.
-   Allows DBAs to clone their environments to the latest application-consistent transaction.