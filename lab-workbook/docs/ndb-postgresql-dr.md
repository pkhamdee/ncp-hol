# Nutanix Database Service (NDB) Lab

# [#](#database-recovery) Database Recovery

Recovering a database quickly and efficiently can save your company man-hours and dollars. NDB uses the Nutanix Cloud Platforms' built-in snapshot function to provide quick recovery of databases, including very large datasets.

## [#](#recovering-the-source-database) Recovering the Source Database

As a Database administrator (DBA), you have been informed that a database has been corrupted and is offline. Due to the amount of corruption, you will need to recover the whole database. Let's get started and see how quickly NDB can help you accomplish that task.

1.  Within NDB, select **\> Data Protection > Time Machines**
    
    This is the **Time Machine** dashboard page, in the first panel you can see the health of your Time Machines and how many of your databaseS are protected.
    
    ![](/ndb/assets/10.b0c62cbb.png)
    
    Note
    
    **Time Machine can have the following statues.**
    
    -   **Critical:** If backups are failing currently
    -   **Warning:** If Backups have failed in the past but recent recovery available
    -   **Healthy:** If all Backups are healthy
    -   **Paused:** If Time machine has been paused by the customer, paused by the system after 500 consecutive log backup failures or if the database is deleted and the time machine is retained.
    
2.  Now select **List** fom the top menu
    
    ![](/ndb/assets/09.7692af78.png)
    
3.  Select the `User##`\-fiestadb\_tm entry.
    
    ![](/ndb/assets/01.57ac5cd0.png)
    
4.  Click **\> Actions > Restore Source Instance**
    
    This is the Time Machine recovery screen. You can recover a database from any point in time the NDB has a snapshot for. Scroll down to **Restore Data To** then choose snapshot from the dropdown, and click **Restore**.
    
    ![](/ndb/assets/02.30cfc7ea.png)
    
    Note
    
    -   Outside of a lab environment, you would be able to choose different days and minutes to restore from depending on the SLA set for that database, allowing you to recover before the database was corrupted.
    -   You can choose to back up the logs before restoring (tail log backup), we skipped that for time considerations.
    
5.  Enter the database name to confirm that you want to overwrite this with another database, then click **Restore** again.
    
    ![](/ndb/assets/03.e64f8d3f.png)
    
6.  Click the blue bar at the top of the screen to see the the restore has started.
    
    ![](/ndb/assets/04.07c646ca.png)
    
    Once you have confirmed the recovery started continue to the [Recovering Data](#recovering-data) section.
    
    Note
    
    Since we chose to skip the log catch-up before the recovery, you may receive an alert indicating that the Time Machine for your user database is unhealthy. To resolve this issue, navigate to **Data Protection > Time Machines**, then select **List**. Choose your **\`User##-fiestadb\_tm** and go to **Actions > Snapshot**. This action will restore the Time Machine to a healthy state, as it is necessary to create a new full snapshot or perform a log catchup following a complete recovery, especially if you did not conduct a tail log backup.
    

## [#](#recovering-data) Recovering Data

In your job as a DBA, you have now received a request to recover a table and some database rows. As we did above, a recovery of the source database will not work here because they do not want to lose any other data already entered. We need to recover the database using a different database VM. Let's see how NDB can handle a recovery situation like this.

1.  Within NDB, select **\> Data Protection > Time Machines** from the drop-down, then click **List** from the top menu.
    
    ![](/ndb/assets/09.7692af78.png)
    
2.  Next select the `User##`\-fiestadb\_TM entry, as you did above.
    
    ![](/ndb/assets/01.57ac5cd0.png)
    
3.  Click **\> Actions > Create a Clone of the PostgreSQL Instance**
    
    At the bottom of the screen select a snapshot from the dropdown, then click **Next**.
    
    ![](/ndb/assets/05.506f8713.png)
    
    Note
    
    Recovering to a cloned copy will allow you to recover the tables or rows that are missing and import them into the affected database.
    
4.  Within the _Database Server VM_ section, fill out the following fields, and click **Next**.
    
    -   **Database Server VM** - Create New Server
    -   **Database Server VM Name** - `User##`\-postgres\_clone
    -   **Compute Profile** - CUSTOM\_EXTRA\_SMALL
    -   **Network Profile** - DEFAULT\_OOB\_POSTGRESQL\_NETWORK
    -   **SSH Public Key** - Select **Text**, and then copy/paste the below into the text field.
    
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoQRdEfm8ZJNGlYLQ2iw08eVk/Wyj0zl3M5KyqKmBTpUaS1uxj0K05HMHaUNP+AeJ63Qa2hI1RJHBJOnV7Dx28/yN7ymQpvO1jWejv/AT/yasC9ayiIT1rCrpHvEDXH9ee0NZ3Dtv91R+8kDEQaUfJLYa5X97+jPMVFC7fWK5PqZRzx+N0bh1izSf8PW0snk3t13DYovHFtlTpzVaYRec/XfgHF9j0032vQDK3svfQqCVzT02NXeEyksLbRfGJwl3UsA1ujQdPgalil0RyyWzCMIabVofz+Czq4zFDFjX+ZPQKZr94/h/6RMBRyWFY5CsUVvw8f+Rq6kW+VTYMvvkv
    ```
    
    ![](/ndb/assets/06.22766092.png)
    
    Note
    
    You could recover to an existing authorized database vm, you are not required to create a new one.
    
5.  Within the _Instance_ section, fill out the following fields, and click **Clone**.
    
    -   **Name** - `User##`\-fiestadb\_clone
    -   **POSTGRES Password** - `nutanix/4u`
    -   **Database Parameter Profile** - DEFAULT\_POSTGRES\_PARAMS
    
    ![](/ndb/assets/07.0c313407.png)
    
    Note
    
    NDB also offers to ability to run scripts or commands both before and after database creation. These can be used to further customize an environment based on specific enterprise needs. Such as creating a sanitized clone from a snapshot.
    
6.  Click the blue bar at the top of the screen to see the the restore has started.
    
    ![](/ndb/assets/08.3d5a9409.png)
    
7.  From the operations page, you can see the progress of the clone creation and the time that it took to recover the source database in our first task.
    
    ![](/ndb/assets/11.54cc82d0.png)
    

So that is a look at some of the options you have for database recovery with NDB. While those operations are finishing, let's move on to [Database Provisioning and Patching](/ndb/provision_postgresql/provision_postgresql.html).