# Nutanix Database Service (NDB) Lab

# [#](#getting-started) Getting Started

Welcome to the _Nutanix Database Service (NDB) Hands-on lab_. This hands-on lab provides first-hand experience of why Nutanix is the ideal platform for database workloads.

Historically, it has been a challenge to virtualize databases because of the high cost of traditional virtualization stacks, and the negative impact that a SAN-based architecture can have on performance. Businesses and their IT departments have constantly fought to balance cost, operational simplicity, and consistent, predictable performance.

Nutanix removes many of these challenges and makes virtualizing business-critical databases much easier. The Nutanix Cloud Platform provides all the features one typically expects in an enterprise SAN, without a typical SAN's physical limitations and bottlenecks. In particular, databases benefit from the following features:

-   Localized I/O, and the use of flash for index and key database files to lower latency.
-   A highly distributed approach that can handle both random and sequential workloads.
-   The ability to add new nodes to scale the infrastructure without either system downtime or performance impact.
-   Built-in data protection, and disaster recovery workflows that simplify backup operations, and business continuity processes.

In addition to solving common infrastructure problems for hosting business-critical applications, NDB also seeks to address many of the key pain points associated with managing databases.

![](/ndb/assets/ndb_new4.d1c366a1.png)

Based on a 2025 research of a large number of North American companies with over 1,000 employees, the following data trends are observed:

-   85% of the organizations have more than 200 database instances in their production environments.
-   80% have more than 10 copies of each database.
-   55% of the total storage capacity is dedicated to accommodating copy data (i.e. copies of production data).
-   45% of database clones require daily refreshes for analytics of dev/test.
-   Copy data will cost IT organizations $100B in 2025.

These numbers maintaining the status quo leads to inefficient usage of both storage, and perhaps worse, an administrator's time . Meet Nutanix Database Service (NDB).

![](/ndb/assets/ndb_new5.dc8bc420.png)

Nutanix Database Service (NDB) provides Database Lifecycle management within your environment. Leveraging the Nutanix Cloud Platform, we are able to take advantage of the power of full stack: storage, compute, and software. NDB abstracts the complexity of database operations, and provides common APIs, CLI, and a consumer-grade GUI experience for multiple database engines. It makes database operations (such as cloning) efficient, thereby driving down the TCO of database management for our customers.

## [#](#explore-ndb) Explore NDB

NDB is distributed as a virtual appliance that can be installed on either AHV or ESXi. For this Bootcamp, a shared NDB server has been deployed on AHV.

When asked for an IP address, user name, or password, refer to the connection detials provided by your Instructor.

1.  Open `HTTP://<NDB IP Address>/` in your browser.
    
2.  Log in using the following credentials:
    
    -   **Username** - `<NDB Username>`
    -   **Password** - `<NDB Password>`
3.  Select **\> Settings** from the menu. Note that NDB has already been configured for your assigned cluster.
    
    ![](/ndb/assets/6.8d49d40d.png)
    
4.  Select **\> Policies > SLAs** from the menu.
    
    NDB has five built-in SLAs: Gold, Silver, Bronze, Brass, and None. SLAs control how the database server is backed up or excluded from being backed up entirely in the case of the _None_ SLA. Backups can be configured with Continuous Protection, Daily, Weekly, Monthly, and Quarterly protection intervals.
    
5.  Select **\> Profiles** from the menu.
    
    Profiles define resources and configurations, making it simple to provision environments and reduce configuration sprawl consistently. For example, a _Compute Profile_ specifies the size of the database server, including details such as vCPUs, cores per vCPU, and memory.
    

## [#](#create-a-maintenance-window) Create A Maintenance Window

A maintenance window policy allows you to set a schedule that is used to automate repeated maintenance tasks such as patching the operating system and database. NDB allows you to create a maintenance window and then associate the maintenance window with a list of database server VMs. You can create a weekly or monthly schedule for a maintenance window. We are creating a maintenance window now so it is available when we do the database provisioing lab.

1.  From within NDB Select **\> Polices > Maintenance Window**
    
2.  Click **Create**
    
3.  Fill out the following fields then click **Create**.
    
    -   **Maintence Window Name** : `user##-maintenance-policy`
    -   **Repeat on**: `Saturday`
    -   **Set time to** : `22:00:00`
    -   **Duration (hours)**: `8`
    
    ![](/ndb/assets/19.14b49166.png)
    
    Note
    
    -   Maintenance windows can be set to run weekly or monthly
    -   Duration sets the time the window is open. When the duration time ends If any database VMs are still running patches they will complete the process, but no new patching processes will be started after the window is closed.
    -   Database VMs running Oracle, MongoDB, and PostgreSQL are supported in maintenance windows for both Operating System and Database patching; MSSQL Database VMs can use maintenance windows for database patching.
    

**Let's get started by seeing how quick and efficient NDB makes [Database Recovery](/ndb/recover_postgresql/recover_postgresql.html).**

## [#](#lab-duration-1-30h) Lab Duration \[1:30h\]

-   \[20m\] Database Recovery
-   \[45m\] Provisioning and patching
-   \[30m\] Creating and Refreshing database clones