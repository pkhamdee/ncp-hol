# Nutanix Unified Storage

# [#](#peer-global-file-service) Peer Global File Service

_The estimated time to complete this lab is 60 minutes._

## [#](#overview) Overview

The explosive growth of unstructured data has driven organizations to seek solutions to efficiently store, share, manage, and protect an ever-growing universe of data while deriving new value and intelligence. Since 1993, Peer Software has focused on these requirements and more by building best-of-breed data management and real-time replication solutions for distributed on-premises and cloud storage environments.

Peer's flagship offering, Peer Global File Service (PeerGFS), features enterprise-class replication technology with integrated file locking and a globally accessible namespace that powers multi-site, multi-vendor, and multi-cloud deployment.

PeerGFS enables fast local data access for users and applications at different locations, protects against version conflicts, makes data highly available, and allows Nutanix Files to co-exist with legacy NAS platforms to ease adoption of Files into existing environments.

Key use cases for combining Peer Software with Nutanix include:

-   **Global File Sharing and Collaboration** - Deliver fast local access to shared project files for distributed teams while ensuring version integrity and high availability.
-   **HA and Load Balancing for User and Application Data** - Enable high availability and load balancing of end user data as well as application data.
-   **Storage Interoperability** - Cross-platform support powers coexistence of Nutanix Files with existing NAS and hybrid cloud storage systems, as well as between file and object formats.
-   **Analysis and Migration** - Analyze existing storage for resource planning, optimization, and migrations. Analysis combined with real-time replication powers minimally disruptive data migrations.

_How does it work?_

![](/unified-storage/assets/integration.1d57e6f8.png)

Working from left to right, users interact with the SMB shares on the Nutanix Files cluster via a public LAN. When SMB activity occurs on the Files cluster through these shares, the Peer Partner Server (referred to as a Peer Agent) is notified via the File Activity Monitoring API from Files. The Peer Agent accesses the updated content via SMB, and then facilitates the flow of data to one or many remote and/or local file servers.

In this lab you will configure Peer Global File Service to create an Active-Active file services solution with Nutanix Files, replicate content from Nutanix Files to Nutanix Objects, and use our File System Analyzer tool to analyze some sample data.

## [#](#lab-setup) Lab Setup

### [#](#files) Files

This lab requires an existing Nutanix Files deployment on your assigned cluster. Details on how to configure Nutanix Files for use with Peer Global File Service can be found in the [Configuring Nutanix Files](#configuring-nutanix-files) section below.

### [#](#peer-vms) Peer VMs

In this exercise, you will be using three shared VMs, all of which should already be available on your assigned cluster.

VM Name

Description

PeerMgmt

This server is running the Peer Management Center

PeerAgent-Files

This server will manage the Nutanix Files cluster

PeerAgent-Win

This Windows File Server will be used as a target for replication

### [#](#configuring-nutanix-files) Configuring Nutanix Files

Peer Global File Service requires both a File Server Admin account as well as REST API access to orchestrate replication to or from Nutanix Files.

1.  Return to your Prism Element browser tab.
    
2.  Select **File Server** from the drop-down navigation.
    
3.  Click on **Launch Files Console** for BootcampFS.
    
4.  Click **Shares** from the top menu, and then **Create a New Share**. Fill out the following fields:
    
    -   **Name** - `User##`**\-Peer**
    -   **Description (Optional)** - Leave blank
    -   **Share Path (Optional)** - Leave blank
    -   **Max Size (Optional)** - Leave blank
    -   **Select Protocol** - SMB
    
    ![](/unified-storage/assets/1.414084fa.png)
    
5.  Click **Next > Next > Create**.
    
6.  Select **Manage Roles** from the _Configuration_ drop-down.
    
7.  Within the _REST API access users_ section, click :fa-plus **New User**. Fill out the following fields.
    
    -   **Username** - `peer`
    -   **Password** - `nutanix/4u`
    
    ![](/unified-storage/assets/2.1068c63b.png)
    
8.  Click
    

### [#](#staging-test-data-on-peeragent-win) Staging Test Data on PeerAgent-Win

The final step of staging the lab is creating some sample data on PeerAgent-Win, which will be acting as a Windows File Server. Peer is capable of replicating between multiple Files clusters, as well as between a mix of Files and other NAS platforms. For this lab, you will be replicating between your Nutanix Files cluster and a Windows File Server.

1.  Return to your **`User##`\-WinTools** remote desktop session.
    
2.  Open **File Explorer** and navigate to `\\PeerAgent-Win\Data`.
    
3.  Create a copy of the **Sample Data** folder. Rename the copy to `User##`**\-Data** as shown below.
    
    ![](/unified-storage/assets/3.903426ab.png)
    

### [#](#connecting-to-the-peer-management-center-web-interface) Connecting to the Peer Management Center Web Interface

The Peer Management Center (PMC) serves as the centralized management component for Peer Global File Service. It does not store any file data but does facilitate communication between locations, so it should be deployed at a location with the best connectivity. A single deployment of PMC can manage 100 or more Agents/file servers.

For this lab, you will be accessing a shared PMC deployment via a web interface.

1.  Open a non-Firefox browser (Chrome, Edge, and Safari will all work) on your `User##`\-WinToolsVM VM or on your laptop.
    
2.  If you are using a browser on your `User##`\-WinTools VM, browse to [https://PeerMgmt:8443/hubopen in new window](https://PeerMgmt:8443/hub)
    
3.  If you are using a browser on your laptop, log in to **Prism Element** (e.g. 10.XX.YY.37) on your Nutanix cluster to find the IP of the PeerMgmt VM, then browse to [https://IP-of-PeerMgmt-Server:8443/hubopen in new window](https://IP-of-PeerMgmt-Server:8443/hub)
    
4.  When prompted to login, use the following credentials:
    
    -   **Username** - `admin`
    -   **Password** - `nutanix/4u`
5.  Once connected, confirm that **PeerAgent-Files** and **PeerAgent-Win** both appear in green in the **Agents** view in the bottom left of the PMC web interface.
    
    ![](/unified-storage/assets/pmc.907774ab.png)
    
    Note
    
    If the two Agent servers are not appearing in the bottom left of the PMC web interface, go into Prism and reboot the VMs named **PeerAgent-Files** and **PeerAgent-Win**.
    

## [#](#introduction-to-peer-global-file-service) Introduction to Peer Global File Service

Peer Global File Service utilizes a job-based configuration engine. Several different job types are available to help tackle different file management challenges. A job represents a combination of:

-   Peer Agents.
-   The file servers that are being monitored by those Agents.
-   A specific share/volume/folder of data on each file server.
-   Various settings tied to replication, synchronization and/or locking.

When creating a new job, you will be prompted by a dialog outlining the different job types and why you would use each type.

Available job types include:

-   **Cloud Backup and Replication** - Real-time replication from enterprise NAS devices to public and private object storage with support for volume-wide point-in-time recovery. Each file is stored as a single, transparent object with optional version tracking.
-   **DFS-N Management** - Manages new and existing Microsoft DFS Namespaces. Can be combined with File Collaboration and/or File Synchronization jobs to automate DFS failover and failback.
-   **File Collaboration** - Real-time synchronization combined with distributed file locking to power global collaboration and project sharing across enterprise NAS platforms, locations, cloud infrastructures, and organizations.
-   **File Replication** - One-way real-time replication from enterprise NAS platforms to any SMB destination.
-   **File Synchronization** - Multi-directional real-time synchronization powering high availability of user and application data across enterprise NAS platforms, locations, cloud infrastructures, and organizations.

## [#](#creating-a-new-file-collaboration-job) Creating a New File Collaboration Job

In this section, we will focus on **File Collaboration**.

1.  In the **PMC Web Interface**, click **File > New Job**.
    
2.  Select **File Collaboration** and click **Create**.
    
    ![](/unified-storage/assets/17.a5e08721.png)
    
3.  Enter `User##`**\-Collab** as the name for the job and click **OK**.
    
    ![](/unified-storage/assets/18.e103dca1.png)
    

### [#](#files-and-peeragent-files) Files and PeerAgent-Files

1.  Click **Add** to begin pairing a Peer Agent with your Nutanix Files cluster.
    
    ![](/unified-storage/assets/19.68daf9fd.png)
    
2.  Select **Nutanix Files** and click **Next**.
    
    ![](/unified-storage/assets/20.bfb08806.png)
    
3.  Select the Agent named **PeerAgent-Files** and click **Next**. This Agent will manage the Files cluster.
    
    ![](/unified-storage/assets/21.fd6c61bf.png)
    
4.  On the **Storage Information** page, you are prompted to enter credentials for accessing the storage device. If another participant sharing your Files cluster has already done the Peer lab, you can select **Existing Credentials** as shown here.
    
    ![](/unified-storage/assets/22a.60afc112.png)
    
    If you are the first participant on this cluster to do the Peer lab, **New Credentials** will be automatically selected. Fill out the following fields:
    
    -   **Nutanix Files Cluster Name** - BootcampFS
        
    -   **Username** - `peer`
        
    -   **Password** - `nutanix/4u`
        
    -   **Peer Agent IP** - **PeerAgent-Files** IP Address
        
        The IP address of the Agent server that will receive real-time notifications from the File Activity Monitoring API built into Files. It is selectable from a drop-down list of available IPs on this Agent server.
        
5.  Click **Validate** to confirm Files can be accessed via API using the provided credentials.
    
    ![](/unified-storage/assets/22.904e1871.png)
    
    Note
    
    Once you enter these credentials, they are reusable when creating new jobs that use this particular Agent. When you create your next job, select **Existing Credentials** on this page to display a list of previously configured credentials.
    
6.  Click **Next**.
    
7.  Click **Browse** to select the share you wish to replicate. You can also navigate to a subfolder below a share.
    
8.  Select your `User##`**\-Peer** share and click **OK**.
    
    ![](/unified-storage/assets/23.d620dade.png)
    
    Note
    
    Peer Global File Service supports the replication of data within nested shares starting with Nutanix Files v3.5.1 and above.
    
    You can only select a single share or folder. You will need to create an additional job for each additional share you wish to replicate.
    
9.  Click **Finish**. You have now completed pairing **PeerAgent-Files** to Nutanix Files.
    
    ![](/unified-storage/assets/24.51b19821.png)
    

### [#](#peeragent-win) PeerAgent-Win

To simplify this lab exercise, a second Peer Agent server running on the same cluster will function as a standard Windows File Server. While Peer can be used to replicate shares between Nutanix Files clusters, one of its key advantages is the ability to work with a mix of NAS platforms. This can help drive adoption of Nutanix Files when only a single site has been refreshed with Nutanix Files, but replication is still required to support collaboration or disaster recovery.

1.  Repeat Steps 1-8 in [Files and PeerAgent-Files](#files-and-peeragent-files) to add **PeerAgent-Win** to the job, making the following changes:
    
    -   **Storage Platform** - Windows File Server
    -   **Management Agent** - PeerAgent-Win
    -   **Path** - `C:\Data\<INITIALS>-Data`
    
    ![](/unified-storage/assets/25.787fadbb.png)
    
2.  Click **Next**.
    

### [#](#completing-collaboration-job-configuration) Completing Collaboration Job Configuration

Peer offers robust functionality for handling the synchronization of NTFS permissions between shares:

-   **Enable synchronizing NTFS security descriptors in real-time**
    
    Select this checkbox if you want changes to file and folder permissions to be replicated to the remote file servers as they occur.
    
-   **Enable synchronizing NTFS security descriptors with master host during initial scan**
    
    Select this if you want the initial scan to look for and replicate any permissions that are not in sync across file servers. This requires selecting a master host to help resolve situations where the engine cannot pick a winner in a permission discrepancy.
    
-   **Synchronize Security Description Options**
    
    (Optional) Select the NTFS permission types you would like to replicate.
    
    -   **Owner**
        
        The NTFS Creator-Owner who owns the object (which is, by default, whoever created it).
        
    -   **DACL**
        
        A Discretionary Access Control List identifies the users and groups that are assigned or denied access permissions on a file or folder.
        
    -   **SACL**
        
        A System Access Control List enables administrators to log attempts to access a secured file or folder. It is used for auditing.
        
-   **File Metadata Conflict Resolution**
    
    If there is a permission discrepancy between two or more sites, the permissions set on the file server tied to the master host will override those on the other file servers.
    

1.  For the purposes of this lab exercise, accept the default configuration and click **Next**.
    
    ![](/unified-storage/assets/26.52427ab1.png)
    
2.  Under **Application Support**, select **Microsoft Office**.
    
    The Peer synchronization and locking engine is automatically optimized to best support any of the selected applications.
    
    ![](/unified-storage/assets/27.3084d843.png)
    
3.  Click **Finish** to complete the job setup.
    

## [#](#starting-a-collaboration-job) Starting a Collaboration Job

Once a job has been created, it must be started to initiate synchronization and file locking.

1.  In the **PMC Web Interface**, under **Jobs**, right-click on your newly created job, and then select **Start**.
    
    ![](/unified-storage/assets/28.bb075931.png)
    
    When the job starts:
    
    -   Connectivity to all Agents and Files clusters (or other NAS devices) is checked.
    -   The real-time monitoring engine is initialized.
    -   A background scan is kicked off to ensure all file servers are in sync with another.
2.  Double-click the job in the **Job** pane to view its runtime information and statistics.
    
    Note
    
    Click **Auto-Update** to have the console regularly refresh as files begin replicating.
    
    ![](/unified-storage/assets/29.d2a9f044.png)
    

## [#](#testing-collaboration) Testing Collaboration

The easiest way to verify synchronization is functioning properly is to open separate File Explorer windows for the Nutanix Files and Windows File Server paths.

Note

Do not test using an Agent server VM. All activity from these servers are automatically filtered to reduce overhead on the Nutanix Files cluster.

1.  Connect to your `User##`\-WinTools VM via RDP using the following credentials:
    
    -   **Username** - `NTNXLAB\Administrator`
    -   **Password** - `nutanix/4u`
2.  Open File Explorer and browse to your `\BootcampFS\User##-Peer` share. Drag this window to the left side of the desktop.
    
    Note that the sample data seeded in the Windows File Server during lab setup has already been replicated to Nutanix Files.
    
    Note
    
    You can also verify the replicated files Within **Prism > File Server**.
    
3.  Open a second File Explorer window and browse to your Windows File Server share, ex., `\PeerAgent-Win\Data\<INITIALS>-Data`. Drag this window to the right side of the desktop.
    
    ![](/unified-storage/assets/30.94cf3ce6.png)
    
4.  In the File Explorer on the left, create a copy of one of the sample data directories by copying and pasting within the root of the share (shown below).
    
    ![](/unified-storage/assets/31.d753e79f.png)
    
    ![](/unified-storage/assets/32.3389272f.png)
    
5.  The changes that are performed on the Nutanix Files share will be sent to its paired Agent; the Agent will then facilitate the replication of these files and folders to the other server (and vice versa).
    
    ![](/unified-storage/assets/33.f16f5df6.png)
    
6.  To test file locking, create a new text file within the root of your `\BootcampFS\User##-Peer` share.
    
    ![](/unified-storage/assets/34.b787fd61.png)
    
7.  Name the file. Within a few seconds, it should appear under your `\PeerAgent-Win\Data\User##-Data` share.
    
    ![](/unified-storage/assets/35.061500b5.png)
    
8.  Open the file under the Nutanix Files share with OpenOffice Writer. Next, open the file with the same name under `\PeerAgent-Win\Data\User##-Data`. You should see the following warning that the file is locked.
    
    ![](/unified-storage/assets/36.43fa0df8.png)
    

Congratulations! You have successfully deployed an Active-Active file share replicated across two file servers. Using Peer, this same approach can be leveraged to support file collaboration across sites, migrations from legacy solutions to Nutanix Files, or disaster recovery for use cases such as VDI, where user data and profiles need to be accessible from multiple sites for business continuity.

## [#](#working-with-nutanix-objects) Working with Nutanix Objects

Peer Global File Service includes the ability to push data from NAS devices into object storage. The same real-time replication technology used to power the collaboration scenario above can also be used to replicate data into Nutanix Objects with optional snapshot capabilities for point-in-time recovery. All objects are replicated in a transparent format that can be immediately used by other apps and services.

This lab section will walk you through the necessary steps to replicate data from Nutanix Files into Nutanix Objects.

### [#](#getting-client-ip-and-credentials-for-nutanix-objects) Getting Client IP and Credentials for Nutanix Objects

In order to replicate data into Objects, you need the Client IP of the object store and need to generate access and secret keys. If you already have this information from a prior lab, you can skip this section and re-use that existing information.

1.  Log in to **Prism Central** (ex., 10.XX.YY.39) on your Nutanix cluster, and then navigate to **Services** > **Objects**.
    
2.  In the **Object Stores** section, find the appropriate object store in the table and note the Client Used IPs.
    
    ![](/unified-storage/assets/clientip.da498452.png)
    
3.  Click on the **Access Keys** section and click **Add People** to begin the process for creating credentials.
    
    ![](/unified-storage/assets/buckets_add_people.ac751801.png)
    
4.  Select **Add people not in Active Directory** and enter your e-mail address.
    
    ![](/unified-storage/assets/buckets_add_people2.d2a7c619.png)
    
5.  Click **Next**.
    
6.  Click **Download Keys** to download a .csv file containing the **Access Key** and **Secret Key**.
    
    ![](/unified-storage/assets/buckets_add_people3.d7cdd84b.png)
    
7.  Click **Close**.
    
8.  Open the file with a text editor.
    
    ![](/unified-storage/assets/buckets_csv_file.848ff96b.png)
    
    Note
    
    Keep the text file open so that you have the access and secret keys readily available for the sections below.
    

### [#](#creating-a-new-cloud-replication-job) Creating a New Cloud Replication Job

In this section, we will focus on creating a **Cloud Backup and Replication** job to replicate data from Nutanix Files into Nutanix Objects.

1.  In the **PMC Web Interface**, click **File > New Job**.
    
    ![](/unified-storage/assets/cloud1.7bc0ad26.png)
    
2.  Select **Cloud Backup and Replication** and click **Create**.
    
3.  Enter `User##`**\-Replication to Objects** as the name for the job and click **OK**.
    
    ![](/unified-storage/assets/cloud2.9b3dd8b7.png)
    
4.  Select **Nutanix Files** and click **Next**.
    
    ![](/unified-storage/assets/cloud3.909257b7.png)
    
5.  Select the Agent named **PeerAgent-Files** and click **Next**. This Agent will manage the Files cluster.
    
    ![](/unified-storage/assets/cloud4.17399f25.png)
    
6.  On the **Storage Information** page, you will see one of two pages. If another participant sharing your Files cluster has already done the Peer lab, you can select their **Existing Credentials** as shown here.
    
    ![](/unified-storage/assets/cloud5.b60a22c0.png)
    
    If you are the first participant on this cluster to do the Peer lab, fill out the following fields:
    
    -   **Nutanix Files Cluster Name** - **BootcampFS**
        
        The NETBIOS name of the Files cluster that will be paired with the Agent selected in the previous step.
        
    -   **Username** - `peer`
        
        This is the Files API account username configured earlier in the lab and MUST be in all lower case.
        
    -   **Password** - `nutanix/4u`
        
        The password associated with the Files API account.
        
    -   **Peer Agent IP** - **PeerAgent-Files** IP Address
        
        The IP address of the Agent server that will receive real-time notifications from the File Activity Monitoring API built into Files. It will be selectable from a dropdown list of available IPs on this Agent server.\*
        
7.  Click **Validate** to confirm Files can be accessed via API using the provided credentials.
    
    ![](/unified-storage/assets/cloud6.86c46dee.png)
    
    Note
    
    Once you enter these credentials, they are reusable when creating new jobs that use this particular Agent. When you create your next job, select **Existing Credentials** on this page to display a list of previously configured credentials.
    
8.  Click **Next**.
    
9.  Select your `User##`**\-Peer** share and click **OK**.
    
    ![](/unified-storage/assets/cloud7.e2e0a894.png)
    
    Note
    
    Peer Global File Service supports the replication of data within nested shares starting with Nutanix Files v3.5.1 and above.
    
    With **Cloud Backup and Replication**, you can select multiple shares and/or folders for a single job.
    
10.  On the **File Filters** page, verify the **Default** filter selected as well as the **Include Files Without Extensions**, and click **Next**.
    
    ![](/unified-storage/assets/cloud8.1e7ae167.png)
    
11.  On the **Destination** page, select **Nutanix Objects** and click **Next**.
    
    ![](/unified-storage/assets/cloud9.ea12c80a.png)
    
12.  On the **Nutanix Objects Credentials** page, fill out the following fields:
    
    -   **Description** -- Name your destination
        
        This is a short name for the Objects credential configuration.
        
    -   **Access Key**
        
        The Access Key associated with the Objects account.
        
    -   **Secret Key**
        
        The Secret Key associated with the Objects account.
        
    -   **Service Point**
        
        The client access IP address or FDQN name of the object store.
        
    
    ![](/unified-storage/assets/cloud10.b961d7ad.png)
    
    Note
    
    Refer to the [Getting Client IP and Credentials for Nutanix Objects](#getting-client-ip-and-credentials-for-nutanix-objects) section above for the appropriate access and secret keys, as well as the Client IP of the object store.
    
13.  Click **Validate** to confirm Objects can be accessed using the provided configuration.
    
    ![](/unified-storage/assets/cloud11.dadb2892.png)
    
14.  Click **OK** in the **Success** window, and then click **Next**.
    
15.  On the **Bucket Details** page, deselect the **Automatically name** checkbox, and then provide a unique bucket name of `User##`**\-peer-objects**.
    
    ![](/unified-storage/assets/cloud12.6598df7c.png)
    
    Note
    
    The bucket name MUST be in all lower case.
    
16.  On the **Replication and Retention Policy** page, select **Existing Policy**, **Continuous Data Protection**, and then click **Next**.
    
    ![](/unified-storage/assets/cloud13.1e1ee49d.png)
    
17.  Click **Next** on the **Miscellaneous Options**, **Email Alerts**, and **SNMP Alerts** pages.
    
18.  Review the configuration on the **Confirmation** screen, and then then click **Finish**.
    
    ![](/unified-storage/assets/cloud14.819316b8.png)
    

### [#](#starting-a-cloud-replication-job) Starting a Cloud Replication Job

Once a job has been created, it must be started to initiate replication.

1.  In the **PMC Web Interface**, right-click on your newly created job, and then select **Start**.
    
    ![](/unified-storage/assets/cloud15.67e6b0da.png)
    
2.  Double-click the job in the **Job** pane to view its runtime information and statistics.
    
    ![](/unified-storage/assets/cloud16.2a6d2191.png)
    
    Note
    
    Click **Auto-Update** to have the console regularly refresh as files begin replicating.
    

### [#](#verifying-replication) Verifying Replication

The easiest way to verify that files have been replicated into Nutanix Objects is to use the Cyberduck tool on your `User##`\-WinTools VM

1.  Connect to your `User##`\-WinTools VM via RDP using the following credentials:
    
    -   **Username** - `NTNXLAB\Administrator`
    -   **Password** - `nutanix/4u`
2.  Launch **Cyberduck** (Click the Window icon > Down Arrow > Cyberduck).
    
    If you are prompted to update Cyberduck, click **Skip This Version**.
    
3.  Click on **Open Connection**.
    
    ![](/unified-storage/assets/buckets_06.3fc32617.png)
    
4.  Select **Amazon S3** from the dropdown list.
    
    ![](/unified-storage/assets/buckets_07.16aa1972.png)
    
5.  Fill out the following fields for the user created earlier, and then click **Connect**:
    
    -   **Server** - _Objects Client Used IP_
    -   **Port** - 443
    -   **Access Key ID** - _Generated When User Created_
    -   **Password (Secret Key)** - _Generated When User Created_
    
    Note
    
    See the [Getting Client IP and Credentials for Nutanix Objects](#getting-client-ip-and-credentials-for-nutanix-objects) section above for the appropriate access and secret keys, as well as the Client IP of the object store.
    
    ![](/unified-storage/assets/buckets_08.61cd8a74.png)
    
6.  Check the **Always Trust** checkbox, and then click **Continue** in the **The certificate is not valid** dialog box.
    
    ![](/unified-storage/assets/invalid_certificate.4ff0bc5d.png)
    
7.  Click **Yes** to continue installing the self-signed certificate.
    
8.  Navigate to the appropriate bucket set above and verify that it contains content.
    
    ![](/unified-storage/assets/cloud19.045da0a3.png)
    

Congratulations! You have successfully setup replication between Nutanix Files and Nutanix Objects! Using Peer, this same approach can be leveraged to support scenarios including coexistence of file data with object-based apps and services as well as point-in-time recovery of enterprise NAS data backed by Objects.

## [#](#analyzing-existing-environments) Analyzing Existing Environments

As the capacity of file server environments increase at a record pace, storage admins often do not know how users and applications are leveraging these file server environments. This fact becomes most evident when it is time to migrate to a new storage platform. The File System Analyzer is a tool from Peer Software that is designed to help partners discover and analyze existing file and folder structures for the purpose of planning and optimization.

The File System Analyzer performs a very fast scan of one or more specified paths, uploads results to Amazon S3, assembles key pieces of information into one or more Excel workbooks, and emails reports with links to access the workbooks.

### [#](#installing-and-running-the-file-system-analyzer) Installing and Running the File System Analyzer

1.  Connect to your `User##`\-WinTools VM via RDP using the following credentials:
    
    -   **Username** - `NTNXLAB\Administrator`
    -   **Password** - `nutanix/4u`
2.  Within the VM, download the File System Analyzer installer: [https://www.peersoftware.com/downloads/fsa/13/FileSystemAnalyzer\_win64.exeopen in new window](https://www.peersoftware.com/downloads/fsa/13/FileSystemAnalyzer_win64.exe)
    
3.  Run the installer and select **Immediate Installation**.
    
    ![](/unified-storage/assets/fsa1.08ca59bb.png)
    
    Once the installation is complete, the File System Analyzer wizard is automatically launched.
    
4.  The **Introduction** screen provides details on information collected and reported by the utility. Click **Next**.
    
    ![](/unified-storage/assets/fsa2.3869d694.png)
    
5.  The **Contact Information** screen collects information used to organize the output of the File System Analyzer and to send the final reports. Fill out the following fields:
    
    -   **Company** -- Enter your company name.
    -   **Location** -- Enter the physical location of the server that is running the File System Analyzer. In multi-site environments, this could be a city or state name. A data center name also works.
    -   **Project** -- Enter a project name or business reason for running this analysis. This (and the Company and Location fields) are used solely to organize the final reports.
    -   **Mode** -- Select the mode of operation to be used -- **General Analysis** or **Migration Preparation**. **Migration Preparation** is useful when preparing for a migration project between storage systems. In addition to collecting standard telemetry on file systems, this mode also offers the option to test performance of both the existing and new storage systems to help gauge potential migration performance and timing. For this lab, we will use **General Analysis**.
    -   **Name/Phone/Title** -- _(Optional)_ Enter your name and contact information.
    -   **Email** -- Enter the email address to which the final reports will be sent. For multiple addresses, enter a comma-separated list.
    -   **Upload Region** -- Select **US**, **EU**, or **APAC** to tell the File System Analyzer which S3 location to use for uploading the final reports.
    
    Note
    
    Be sure to enter your own details into the wizard page shown below. Otherwise, the final report will not be sent to you.
    
    ![](/unified-storage/assets/fsa3.9e98b84e.png)
    
6.  Click **Next**.
    
    The File System Analyzer can be configured to scan one or more paths. These paths can be local (ex., `D:\MyData`) or a remote UNC Path (ex., `\files01\homes1`).
    
7.  Add the following paths:
    
    -   `C:\` - The local C: drive of `User##`\-WinTools VM
    -   `\BootcampFS\<Your Share Name>\` - A share previously created on Nutanix Files
    
    ![Click the Search button and enter the name of a file server if you wish to discover the available shares on that file server. You can also right-click within the dialog and select Check All to automatically add all discovered shares.](/unified-storage/assets/fsa4.7772f6be.png)
    
    ![Selecting the Log totals by owner option will poke every file and folder within the selected scan path(s) for its owner. This owner information will be tallied by bytes, files, and folders and included in the final report.](/unified-storage/assets/fsa4a.5309794d.png)
    
8.  Click **Next**.
    
    Click the **Start** button to begin scanning the entered paths. When all scans, analyses, and uploads are complete, you will see a status that is similar to the following:
    
    ![](/unified-storage/assets/fsa5.48a6bb12.png)
    
9.  File System Analyzer will also email the report to all configured addresses. To view the full report, click the hyperlink(s) listed under **Detailed Reports** in the email. If multiple paths were scanned, you will also see a link to a cumulative report across all paths.
    
    ![](/unified-storage/assets/fsa6.8d5edfaa.png)
    
    Note
    
    Report download links are active for **24 hours** only. Contact Peer Software to access any expired reports.
    
    Some systems may open these workbooks in a protected mode, displaying this message in Excel:
    
    ![](/unified-storage/assets/fsa8.73acbf48.png)
    
    If you see this message at the top of Excel, click **Enable Editing** to fully open the workbook. If you do not do this, the pivot tables and charts will not load properly.
    

### [#](#summary-reports) Summary Reports

Summary reports contain overall statistical and historical information across all paths that have been selected to be scanned. When you open a summary report, you are greeted with a worksheet like this:

![](/unified-storage/assets/fsa7.8741be91.png)

Each summary report may contain some or all of the following worksheets:

-   **InfoSheet** -- Details about this specific run. This page will also show Total Bytes formatted in both decimal (1 KB is 1,000 bytes) and binary (1 KiB is 1,024 bytes) forms.
-   **CollectiveResults** -- A list of all paths scanned along with high-level statistics for each.
-   **History-Bytes** -- Contains historical changes in bytes for each time each path is scanned.
-   **History-Files** -- Contains historical changes in total number of files for each time each path is scanned.
-   **History-Folders** -- Contains historical changes in total numbers of folders for each time each path is scanned.

Note

History worksheets will only appear after running multiple scans.

### [#](#volume-reports) Volume Reports

Volume reports give more detailed information about a specific path that has been scanned. When you open a volume report, you are greeted with a worksheet like this:

![](/unified-storage/assets/fsa7a.d03643f8.png)

Each volume report may contain some or all of the following worksheets:

-   **Overview** -- A series of pivot tables and charts showing high-level statistics about the path that was scanned.
-   **InfoSheet** -- Details about this specific scan. This page will also show Total Bytes formatted in both decimal (1 KB is 1,000 bytes) and binary (1 KiB is 1,024 bytes) forms.
-   **OverallStats** -- Overall statistics for the folder that was scanned. This includes total bytes, files, folders, etc.
-   **Analysis** -- Includes a pivot table and a pair of charts highlighting additional statistics about the path that was scanned.
-   **History** -- Shows statistics from each scan of this volume.
-   **HistoryCharts** -- Contains charts showing historical changes in files, folders, and bytes for this volume.
-   **HighSubFolderCounts** -- A list of all folders containing more than 100 child directories.
-   **HighByteCounts** -- A list of all folders containing more than 10GB of child file data.
-   **HighFileCounts** -- A list of all folders containing more than 10,000 child files.
-   **LargeFiles** -- A list of all discovered files that are 10GB or larger.
-   **DeepPaths** -- A list of all discovered folder paths that are 15 levels deep or deeper.
-   **LongPaths** -- A list of all discovered folder paths that are 256 characters or longer.
-   **ReparsePointsSummary** -- A summary of all reparse points discovered, regardless of file or folder.
-   **ReparsePoints** -- A list of all folder reparse points discovered.
-   **TimeAnalysis** -- A breakdown of total files, folders, and bytes by age.
-   **LastModifiedAnalysis** -- A view of all files, folders, and bytes modified each hour for the past year. These numbers are then totaled and averaged to show files, folders, and bytes modified by: day of week; month; hour of the day; day of month; and day of year.
-   **CreatedAnalysis** -- A view of all files, folders, and bytes created each hour for the past year. These numbers are then totaled and averaged to show files, folders, and bytes created by day of week, month, hour of the day, day of month, and day of year.
-   **LastAccessedAnalysis** -- A view of all files, folders, and bytes accessed each hour for the past year. These numbers are then totaled and averaged to show files, folders, and bytes accessed by: day of week; month; hour of the day; day of month; and day of year.
-   **TLDAnalysis** - A list of each folder immediately under a specified path with statistics for each of these subfolders. In a user home directory environment, each of these subfolders should represent a different user.
-   **TopTLDsByTotals** -- A series of pivot tables and charts showing the top ten top-level directories based on total bytes used, total files, and total folders.
-   **TopTLDsByLastModBytes** -- A pivot table and chart showing top 10 top-level directories based on most bytes modified in the past year.
-   **TopTLDsByLastModFiles** -- A pivot table and chart showing top 10 top-level directories based on most files modified in the past year.
-   **LegacyTLDs** -- A list of all top-level directories that do not contain any files modified in the past 365 days.
-   **TreeDepth** -- A tally of bytes, folders, and files found at each depth level of the folder structure. For customers doing a pre-migration analysis, depths that appear as green are good candidates for PeerSync Migration's tree depth setting.
-   **FileExtInfo** -- A list of all discovered extensions, including pivot tables sorted by total bytes and total files.
-   **FileAttributes** -- A summary of all file and folder attributes found.
-   **SmallFileAnalysis** -- A list of all files discovered below a certain size. This page is useful for estimating the storage impact of small files on storage platforms that have large minimum file sizes on disk.
-   **SIDCache** -- A list of all the owners and SID strings that have been discovered.

Note

History worksheets will only appear after running multiple scans.

Here is a sample of the **LastModifiedAnalysis** page mentioned above:

![](/unified-storage/assets/fsa7b.32b46f08.png)

## [#](#integrating-with-microsoft-dfs-namespace) Integrating with Microsoft DFS Namespace

Peer Global File Service includes the ability to create and manage Microsoft DFS Namespaces (DFS-N). When this DFS-N integration is combined with its real-time replication and file locking engine, PeerGFS powers a true global namespace that spans locations and storage devices.

As part of its DFS namespace management capabilities, PeerGFS also automatically redirects users away from a failed file server. When that failed server comes back online, PeerGFS brings this file server back in-sync, and then re-enables user access to it. \*This is an essential Disaster Recovery feature for any deployment looking to leverage Nutanix Files for user profile and user data shares for VDI environments.

The following screenshot shows the PMC interface with a DFS Namespace under management.

![](/unified-storage/assets/dfsn.43c6ff26.png)

## [#](#takeaways) Takeaways

-   Peer Global File Service is the only solution which can provide Active-Active replication for Nutanix Files clusters.
-   Peer also supports multiple legacy NAS platforms and supports replication within mixed environments. This helps ease adoption of and migration to Nutanix Files.
-   Peer can directly manage Microsoft Distributed File Services (DFS) namespaces, allowing multiple file servers to be presented through a single namespace. This is a key component for supporting true Active-Active DR solutions for file sharing.
-   Peer can replicate files from Nutanix Files and other NAS platforms into Nutanix Objects with optional snapshot capabilities for point-in-time recovery. All objects are in a transparent format that can be immediately used by other apps and services.
-   Peer offers tools for analyzing existing file servers to help with resource planning, optimization, and minimally disruptive migrations.