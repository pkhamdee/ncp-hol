# Peer Global File Service

_เวลาที่ใช้ในการทำแล็บนี้ประมาณ 60 นาที_

## Overview

การเติบโตอย่างรวดเร็วของ unstructured data ทำให้องค์กรต่างๆ ต้องหาวิธีการจัดเก็บ แชร์ จัดการ และปกป้องข้อมูลที่เพิ่มขึ้นอย่างต่อเนื่อง พร้อมทั้งดึงมูลค่าและข้อมูลเชิงลึกใหม่ๆ ออกมา ตั้งแต่ปี 1993, Peer Software มุ่งเน้นไปที่ความต้องการเหล่านี้และอื่นๆ อีกมากมายด้วยการสร้างโซลูชันด้านการจัดการข้อมูลและการจำลองข้อมูลแบบเรียลไทม์ (real-time replication) ที่ดีที่สุดในระดับเดียวกัน สำหรับสภาพแวดล้อม storage แบบกระจายที่อยู่ใน on-premises และ cloud

ผลิตภัณฑ์หลักของ Peer คือ Peer Global File Service (PeerGFS) ซึ่งมีเทคโนโลยี replication ระดับ enterprise พร้อมด้วย file locking ในตัว และ namespace ที่เข้าถึงได้จากทั่วโลก ซึ่งรองรับการใช้งานแบบ multi-site, multi-vendor และ multi-cloud deployment

PeerGFS ช่วยให้ผู้ใช้และแอปพลิเคชันใน location ต่างๆ สามารถเข้าถึงข้อมูล local ได้อย่างรวดเร็ว ป้องกันปัญหา version conflicts, ทำให้ข้อมูลมี high availability และช่วยให้ Nutanix Files สามารถทำงานร่วมกับระบบ NAS เดิมที่มีอยู่ เพื่อให้ง่ายต่อการนำ Files ไปใช้ในสภาพแวดล้อมปัจจุบัน

Key use cases สำหรับการใช้งานรวมกันระหว่าง Peer Software และ Nutanix ได้แก่:

-   **Global File Sharing and Collaboration** - ส่งมอบการเข้าถึงไฟล์โปรเจกต์ที่แชร์ไว้แบบ local อย่างรวดเร็วสำหรับทีมที่กระจายอยู่หลายที่ พร้อมรับประกันความสมบูรณ์ของ version และ high availability
-   **HA and Load Balancing for User and Application Data** - เปิดใช้งาน high availability และ load balancing สำหรับข้อมูลของผู้ใช้ (end user data) และข้อมูลแอปพลิเคชัน (application data)
-   **Storage Interoperability** - การรองรับข้ามแพลตฟอร์มช่วยให้ Nutanix Files สามารถทำงานร่วมกับระบบ NAS เดิมและ hybrid cloud storage ได้ รวมถึงการทำงานระหว่างรูปแบบ file และ object
-   **Analysis and Migration** - วิเคราะห์ storage ที่มีอยู่เพื่อการวางแผนทรัพยากร (resource planning), การเพิ่มประสิทธิภาพ (optimization), และ migrations การวิเคราะห์เมื่อรวมกับ real-time replication จะช่วยให้ทำ data migrations ได้โดยกระทบการทำงานน้อยที่สุด

_มันทำงานอย่างไร?_

![](/images/integration.1d57e6f8.png)

การทำงานจากซ้ายไปขวา ผู้ใช้โต้ตอบกับ SMB shares บน Nutanix Files cluster ผ่าน public LAN เมื่อมีกิจกรรม SMB เกิดขึ้นบน Files cluster ผ่าน share เหล่านี้ Peer Partner Server (หรือเรียกว่า Peer Agent) จะได้รับการแจ้งเตือนผ่าน File Activity Monitoring API จาก Files จากนั้น Peer Agent จะเข้าถึงเนื้อหาที่อัปเดตผ่าน SMB และอำนวยความสะดวกในการไหลของข้อมูลไปยัง remote และ/หรือ local file servers หนึ่งแห่งหรือหลายแห่ง

ในแล็บนี้ คุณจะได้กำหนดค่า Peer Global File Service เพื่อสร้างโซลูชัน Active-Active file services กับ Nutanix Files, ทำการ replicate เนื้อหาจาก Nutanix Files ไปยัง Nutanix Objects และใช้เครื่องมือ File System Analyzer ของเราเพื่อวิเคราะห์ sample data บางส่วน

## Lab Setup

### Files

แล็บนี้ต้องการการติดตั้ง Nutanix Files ที่มีอยู่แล้วบน cluster ที่คุณได้รับมอบหมาย รายละเอียดเกี่ยวกับวิธีกำหนดค่า Nutanix Files สำหรับใช้กับ Peer Global File Service สามารถพบได้ในส่วน [Configuring Nutanix Files](#configuring-nutanix-files) ด้านล่าง

### Peer VMs

ในแบบฝึกหัดนี้ คุณจะใช้ shared VMs จำนวน 3 เครื่อง ซึ่งทั้งหมดควรมีอยู่แล้วบน cluster ที่คุณได้รับมอบหมาย

VM Name | Description
--- | ---
PeerMgmt | เซิร์ฟเวอร์นี้กำลังรัน Peer Management Center
PeerAgent-Files | เซิร์ฟเวอร์นี้จะทำหน้าที่จัดการ Nutanix Files cluster
PeerAgent-Win | เซิร์ฟเวอร์ Windows File Server นี้จะถูกใช้เป็น target สำหรับการ replication

### Configuring Nutanix Files

Peer Global File Service ต้องการทั้งบัญชี File Server Admin และการเข้าถึง REST API เพื่อจัดการการ replication ไปยังหรือจาก Nutanix Files

1.  กลับไปที่แท็บเบราว์เซอร์ Prism Element ของคุณ
    
2.  เลือก **File Server** จากเมนู drop-down navigation
    
3.  คลิกที่ **Launch Files Console** สำหรับ BootcampFS
    
4.  คลิก **Shares** จากเมนูด้านบน แล้วเลือก **Create a New Share** กรอกข้อมูลในฟิลด์ต่อไปนี้:
    
    -   **Name** - `User##`**\-Peer**
    -   **Description (Optional)** - เว้นว่างไว้
    -   **Share Path (Optional)** - เว้นว่างไว้
    -   **Max Size (Optional)** - เว้นว่างไว้
    -   **Select Protocol** - SMB
    
    ![](/images/1.414084fa.png)
    
5.  คลิก **Next > Next > Create**
    
6.  เลือก **Manage Roles** จาก drop-down _Configuration_
    
7.  ภายในส่วน _REST API access users_ คลิก :fa-plus **New User** กรอกข้อมูลในฟิลด์ต่อไปนี้:
    
    -   **Username** - `peer`
    -   **Password** - `nutanix/4u`
    
    ![](/images/2.1068c63b.png)
    
8.  คลิก Save
    

### Staging Test Data on PeerAgent-Win

ขั้นตอนสุดท้ายของการเตรียมแล็บคือการสร้าง sample data บน PeerAgent-Win ซึ่งจะทำหน้าที่เป็น Windows File Server. Peer สามารถทำการ replicate ระหว่าง Files clusters หลายคลัสเตอร์ได้ รวมถึงการใช้งานแบบผสมระหว่าง Files และ NAS platforms อื่นๆ สำหรับแล็บนี้ คุณจะทำการ replicate ระหว่าง Nutanix Files cluster ของคุณและ Windows File Server

1.  กลับไปที่เซสชัน remote desktop **`User##`\-WinTools** ของคุณ
    
2.  เปิด **File Explorer** และนำทางไปยัง `\\PeerAgent-Win\Data`
    
3.  สร้างสำเนาของโฟลเดอร์ **Sample Data** เปลี่ยนชื่อสำเนาเป็น `User##`**\-Data** ตามที่แสดงด้านล่าง
    
    ![](/images/3.903426ab.png)
    

### Connecting to the Peer Management Center Web Interface

Peer Management Center (PMC) ทำหน้าที่เป็น centralized management component สำหรับ Peer Global File Service โดยจะไม่ได้จัดเก็บ file data ใดๆ แต่จะช่วยอำนวยความสะดวกในการสื่อสารระหว่าง location ต่างๆ ดังนั้นจึงควรถูก deploy ใน location ที่มีการเชื่อมต่อที่ดีที่สุด การ deploy PMC เพียงตัวเดียวสามารถจัดการ Agents/file servers ได้ 100 เครื่องหรือมากกว่านั้น

สำหรับแล็บนี้ คุณจะเข้าถึง shared PMC deployment ผ่านทาง web interface

1.  เปิดเบราว์เซอร์ที่ไม่ใช่ Firefox (Chrome, Edge และ Safari สามารถใช้งานได้) บน VM `User##`\-WinTools ของคุณหรือบนแล็ปท็อป
    
2.  หากคุณใช้เบราว์เซอร์บน VM `User##`\-WinTools ให้ไปที่ [https://PeerMgmt:8443/hub](https://PeerMgmt:8443/hub)
    
3.  หากคุณใช้เบราว์เซอร์บนแล็ปท็อป ให้ล็อกอินเข้าสู่ **Prism Element** (เช่น 10.XX.YY.37) บน Nutanix cluster ของคุณเพื่อค้นหา IP ของ VM PeerMgmt จากนั้นไปที่ [https://IP-of-PeerMgmt-Server:8443/hub](https://IP-of-PeerMgmt-Server:8443/hub)
    
4.  เมื่อได้รับ prompt ให้ล็อกอิน ให้ใช้ credentials ต่อไปนี้:
    
    -   **Username** - `admin`
    -   **Password** - `nutanix/4u`

5.  เมื่อเชื่อมต่อแล้ว ให้ยืนยันว่า **PeerAgent-Files** และ **PeerAgent-Win** ทั้งสองปรากฏเป็นสีเขียวในมุมมอง **Agents** ที่ด้านซ้ายล่างของ PMC web interface
    
    ![](/images/pmc.907774ab.png)
    
    !!! note
        หาก Agent servers ทั้งสองไม่ปรากฏที่ด้านซ้ายล่างของ PMC web interface ให้เข้าไปที่ Prism และทำการ reboot VMs ที่ชื่อ **PeerAgent-Files** และ **PeerAgent-Win**
    

## Introduction to Peer Global File Service

Peer Global File Service ใช้ job-based configuration engine โดยมี job types ต่างๆ หลายประเภทเพื่อให้ความช่วยเหลือในการจัดการความท้าทายด้าน file management ที่แตกต่างกัน job หมายถึงการรวมกันของ:

-   Peer Agents
-   file servers ที่ถูกตรวจสอบโดย Agents เหล่านั้น
-   share/volume/folder ข้อมูลเฉพาะบนแต่ละ file server
-   การตั้งค่าต่างๆ ที่ผูกกับ replication, synchronization และ/หรือ locking

เมื่อสร้าง job ใหม่ คุณจะพบกับ dialog ที่สรุป job types ต่างๆ และเหตุผลที่คุณควรใช้แต่ละประเภท

job types ที่มีให้ใช้งาน ได้แก่:

-   **Cloud Backup and Replication** - Real-time replication จาก enterprise NAS devices ไปยัง public และ private object storage โดยรองรับการทำ volume-wide point-in-time recovery แต่ละไฟล์จะถูกเก็บเป็น object เดี่ยวๆ แบบโปร่งใส (transparent) พร้อมกับ version tracking แบบ optional
-   **DFS-N Management** - จัดการ Microsoft DFS Namespaces ทั้งใหม่และที่มีอยู่ สามารถรวมกับ File Collaboration และ/หรือ File Synchronization jobs เพื่อทำ DFS failover และ failback แบบอัตโนมัติ
-   **File Collaboration** - Real-time synchronization ร่วมกับ distributed file locking เพื่อสนับสนุน global collaboration และ project sharing ข้าม enterprise NAS platforms, locations, cloud infrastructures, และองค์กร
-   **File Replication** - One-way real-time replication จาก enterprise NAS platforms ไปยัง SMB destination ใดๆ
-   **File Synchronization** - Multi-directional real-time synchronization เพื่อสนับสนุน high availability ของข้อมูลผู้ใช้และแอปพลิเคชันข้าม enterprise NAS platforms, locations, cloud infrastructures, และองค์กร

## Creating a New File Collaboration Job

ในส่วนนี้ เราจะมุ่งเน้นไปที่ **File Collaboration**

1.  ใน **PMC Web Interface**, คลิก **File > New Job**.
    
2.  เลือก **File Collaboration** แล้วคลิก **Create**.
    
    ![](/images/17.a5e08721.png)
    
3.  ป้อน `User##`**\-Collab** เป็นชื่อสำหรับ job แล้วคลิก **OK**.
    
    ![](/images/18.e103dca1.png)
    

### Files and PeerAgent-Files

1.  คลิก **Add** เพื่อเริ่มทำการจับคู่ Peer Agent กับ Nutanix Files cluster ของคุณ
    
    ![](/images/19.68daf9fd.png)
    
2.  เลือก **Nutanix Files** แล้วคลิก **Next**.
    
    ![](/images/20.bfb08806.png)
    
3.  เลือก Agent ที่ชื่อ **PeerAgent-Files** แล้วคลิก **Next**. Agent นี้จะทำหน้าที่จัดการ Files cluster
    
    ![](/images/21.fd6c61bf.png)
    
4.  ในหน้า **Storage Information** คุณจะได้รับ prompt ให้ป้อน credentials สำหรับการเข้าถึง storage device หากผู้เข้าร่วมคนอื่นที่แชร์ Files cluster ของคุณได้ทำแล็บ Peer ไปแล้ว คุณสามารถเลือก **Existing Credentials** ตามที่แสดงที่นี่
    
    ![](/images/22a.60afc112.png)
    
    หากคุณเป็นผู้เข้าร่วมคนแรกบน cluster นี้ที่ทำแล็บ Peer จะมีการเลือก **New Credentials** ให้โดยอัตโนมัติ กรอกข้อมูลในฟิลด์ต่อไปนี้:
    
    -   **Nutanix Files Cluster Name** - BootcampFS
        
    -   **Username** - `peer`
        
    -   **Password** - `nutanix/4u`
        
    -   **Peer Agent IP** - **PeerAgent-Files** IP Address
        
        IP address ของ Agent server ที่จะรับ real-time notifications จาก File Activity Monitoring API ที่มาพร้อมกับ Files โดยสามารถเลือกได้จาก drop-down list ของ IPs ที่ใช้งานได้บน Agent server นี้
        
5.  คลิก **Validate** เพื่อยืนยันว่า Files สามารถเข้าถึงได้ผ่าน API โดยใช้ credentials ที่ให้ไว้
    
    ![](/images/22.904e1871.png)
    
    !!! note
        เมื่อคุณป้อน credentials เหล่านี้ พวกมันจะถูกนำมาใช้ซ้ำได้เมื่อสร้าง jobs ใหม่ที่ใช้ Agent เฉพาะเจาะจงตัวนี้ เมื่อคุณสร้าง job ถัดไป ให้เลือก **Existing Credentials** ในหน้านี้เพื่อแสดงรายการ credentials ที่เคยถูกกำหนดค่าไว้แล้ว
    
6.  คลิก **Next**.
    
7.  คลิก **Browse** เพื่อเลือก share ที่คุณต้องการทำการ replicate คุณสามารถนำทางไปยัง subfolder ภายใต้ share ได้ด้วย
    
8.  เลือก `User##`**\-Peer** share ของคุณแล้วคลิก **OK**.
    
    ![](/images/23.d620dade.png)
    
    !!! note
        Peer Global File Service รองรับการ replication ของข้อมูลภายใน nested shares โดยเริ่มตั้งแต่ Nutanix Files v3.5.1 ขึ้นไป คุณสามารถเลือกได้เพียง share หรือ folder เดียวเท่านั้น คุณจะต้องสร้าง job เพิ่มเติมสำหรับแต่ละ share เพิ่มเติมที่คุณต้องการทำการ replicate
    
9.  คลิก **Finish**. คุณทำการจับคู่ **PeerAgent-Files** ไปยัง Nutanix Files เสร็จสมบูรณ์แล้ว
    
    ![](/images/24.51b19821.png)
    

### PeerAgent-Win

เพื่อลดความซับซ้อนในแบบฝึกหัดแล็บนี้ Peer Agent server ตัวที่สองที่ทำงานบน cluster เดียวกันจะทำหน้าที่เป็น standard Windows File Server แม้ว่า Peer จะสามารถนำมาใช้ในการ replicate shares ระหว่าง Nutanix Files clusters ได้ แต่หนึ่งในข้อได้เปรียบหลักคือความสามารถในการทำงานร่วมกับ NAS platforms แบบผสมผสาน สิ่งนี้สามารถช่วยผลักดันการนำ Nutanix Files ไปใช้ได้ในกรณีที่มีเพียง site เดียวที่ได้รับการ refresh เป็น Nutanix Files แต่ยังคงต้องการ replication เพื่อรองรับ collaboration หรือ disaster recovery

1.  ทำซ้ำขั้นตอนที่ 1-8 ใน [Files and PeerAgent-Files](#files-and-peeragent-files) เพื่อเพิ่ม **PeerAgent-Win** เข้าไปใน job โดยทำการเปลี่ยนแปลงต่อไปนี้:
    
    -   **Storage Platform** - Windows File Server
    -   **Management Agent** - PeerAgent-Win
    -   **Path** - `C:\Data\<INITIALS>-Data`
    
    ![](/images/25.787fadbb.png)
    
2.  คลิก **Next**.
    

### Completing Collaboration Job Configuration

Peer มีฟังก์ชันการทำงานที่แข็งแกร่งสำหรับการจัดการ synchronization ของ NTFS permissions ระหว่าง shares:

-   **Enable synchronizing NTFS security descriptors in real-time**
    
    เลือกช่องทำเครื่องหมายนี้หากคุณต้องการให้การเปลี่ยนแปลงสิทธิ์ของไฟล์และโฟลเดอร์ถูก replicate ไปยัง remote file servers ในขณะที่มันเกิดขึ้น
    
-   **Enable synchronizing NTFS security descriptors with master host during initial scan**
    
    เลือกตัวเลือกนี้หากคุณต้องการให้ initial scan ทำการค้นหาและ replicate สิทธิ์ใดๆ ที่ไม่ได้ in sync ข้าม file servers สิ่งนี้จำเป็นต้องมีการเลือก master host เพื่อช่วยแก้ไขสถานการณ์ที่ engine ไม่สามารถตัดสินได้เมื่อมีความขัดแย้งของสิทธิ์
    
-   **Synchronize Security Description Options**
    
    (Optional) เลือกประเภท NTFS permission ที่คุณต้องการทำการ replicate
    
    -   **Owner**
        
        NTFS Creator-Owner ซึ่งเป็นเจ้าของ object (ตามค่าเริ่มต้น คือผู้ที่สร้างมันขึ้นมา)
        
    -   **DACL**
        
        Discretionary Access Control List ระบุผู้ใช้และ groups ที่ได้รับอนุญาตหรือปฏิเสธ access permissions ในไฟล์หรือโฟลเดอร์
        
    -   **SACL**
        
        System Access Control List ช่วยให้ administrators สามารถ log ความพยายามในการเข้าถึงไฟล์หรือโฟลเดอร์ที่มีความปลอดภัยได้ โดยถูกนำมาใช้สำหรับการ auditing
        
-   **File Metadata Conflict Resolution**
    
    หากมี permission discrepancy ระหว่าง 2 sites หรือมากกว่านั้น permissions ที่ตั้งไว้ใน file server ที่ผูกกับ master host จะ override สิทธิ์บน file servers อื่นๆ
    

1.  สำหรับวัตถุประสงค์ของแบบฝึกหัดแล็บนี้ ให้ยอมรับการกำหนดค่าเริ่มต้น (default configuration) แล้วคลิก **Next**.
    
    ![](/images/26.52427ab1.png)
    
2.  ภายใต้ **Application Support**, เลือก **Microsoft Office**.
    
    Peer synchronization and locking engine จะถูก optimized อัตโนมัติเพื่อรองรับแอปพลิเคชันที่ถูกเลือกอย่างดีที่สุด
    
    ![](/images/27.3084d843.png)
    
3.  คลิก **Finish** เพื่อเสร็จสิ้นการตั้งค่า job
    

## Starting a Collaboration Job

เมื่อสร้าง job แล้ว จะต้องถูก start เพื่อเริ่มต้น synchronization และ file locking

1.  ใน **PMC Web Interface**, ภายใต้ **Jobs**, ให้คลิกขวาที่ job ที่เพิ่งสร้างใหม่ของคุณ แล้วเลือก **Start**.
    
    ![](/images/28.bb075931.png)
    
    เมื่อ job เริ่มทำงาน:
    
    -   การเชื่อมต่อไปยัง Agents และ Files clusters (หรือ NAS devices อื่นๆ) ทั้งหมดจะถูกตรวจสอบ
    -   real-time monitoring engine จะถูกเริ่มต้น (initialize)
    -   background scan จะถูกสั่งให้ทำงานเพื่อยืนยันว่า file servers ทั้งหมด in sync ซึ่งกันและกัน
2.  ดับเบิลคลิก job ใน pane **Job** เพื่อดู runtime information และ statistics ของมัน
    
    !!! note
        คลิก **Auto-Update** เพื่อให้ console รีเฟรชตัวเองอย่างสม่ำเสมอเมื่อไฟล์เริ่มทำการ replicate
    
    ![](/images/29.d2a9f044.png)
    

## Testing Collaboration

วิธีที่ง่ายที่สุดในการยืนยันว่า synchronization ทำงานได้อย่างถูกต้องคือการเปิดหน้าต่าง File Explorer แยกกันสำหรับเส้นทาง Nutanix Files และ Windows File Server

!!! note
    อย่าทดสอบโดยใช้ Agent server VM กิจกรรมทั้งหมดจากเซิร์ฟเวอร์เหล่านี้จะถูกกรองทิ้ง (filtered) โดยอัตโนมัติเพื่อลด overhead บน Nutanix Files cluster

1.  เชื่อมต่อไปยัง `User##`\-WinTools VM ของคุณผ่าน RDP โดยใช้ credentials ต่อไปนี้:
    
    -   **Username** - `NTNXLAB\Administrator`
    -   **Password** - `nutanix/4u`

2.  เปิด File Explorer และไปที่ share `\BootcampFS\User##-Peer` ของคุณ ลากหน้าต่างนี้ไปที่ด้านซ้ายของหน้าจอเดสก์ท็อป
    
    โปรดสังเกตว่า sample data ที่ถูกเตรียมไว้ (seeded) ใน Windows File Server ระหว่างการตั้งค่าแล็บได้ถูก replicate ไปยัง Nutanix Files แล้ว
    
    !!! note    
        คุณสามารถตรวจสอบไฟล์ที่ถูก replicate ภายใน **Prism > File Server** ได้เช่นกัน
    
3.  เปิดหน้าต่าง File Explorer ที่สองและไปที่ Windows File Server share ของคุณ เช่น `\PeerAgent-Win\Data\<INITIALS>-Data` ลากหน้าต่างนี้ไปที่ด้านขวาของหน้าจอเดสก์ท็อป
    
    ![](/images/30.94cf3ce6.png)
    
4.  ใน File Explorer ด้านซ้าย ให้สร้างสำเนาของหนึ่งในไดเรกทอรี sample data โดยการคัดลอกและวางภายใน root ของ share
    
    ![](/images/31.d753e79f.png)
    
    ![](/images/32.3389272f.png)
    
5.  การเปลี่ยนแปลงที่ทำบน Nutanix Files share จะถูกส่งไปยัง Agent ที่จับคู่ไว้; จากนั้น Agent จะจัดการการ replication ของไฟล์และโฟลเดอร์เหล่านี้ไปยังเซิร์ฟเวอร์อีกเครื่อง (และในทางกลับกัน)
    
    ![](/images/33.f16f5df6.png)
    
6.  เพื่อทดสอบ file locking ให้สร้างไฟล์ text ใหม่ภายใน root ของ `\BootcampFS\User##-Peer` share ของคุณ
    
    ![](/images/34.b787fd61.png)
    
7.  ตั้งชื่อไฟล์ ภายในไม่กี่วินาที มันควรจะปรากฏอยู่ภายใต้ share `\PeerAgent-Win\Data\User##-Data` ของคุณ
    
    ![](/images/35.061500b5.png)
    
8.  เปิดไฟล์ภายใต้ Nutanix Files share ด้วย OpenOffice Writer จากนั้น เปิดไฟล์ชื่อเดียวกันภายใต้ `\PeerAgent-Win\Data\User##-Data` คุณควรจะเห็นคำเตือนดังต่อไปนี้ว่าไฟล์ถูกล็อก (locked)
    
    ![](/images/36.43fa0df8.png)
    

ขอแสดงความยินดี! คุณประสบความสำเร็จในการ deploy ระบบ Active-Active file share ที่ทำการ replicate ข้าม file servers 2 เครื่อง การใช้ Peer ทำให้แนวทางเดียวกันนี้สามารถนำไปใช้เพื่อรองรับ file collaboration ข้ามไซต์, การทำ migrations จาก legacy solutions ไปยัง Nutanix Files, หรือการทำ disaster recovery สำหรับ use cases เช่น VDI, ซึ่ง user data และ profiles จำเป็นต้องสามารถเข้าถึงได้จากหลายไซต์เพื่อรักษาความต่อเนื่องทางธุรกิจ (business continuity)

## Working with Nutanix Objects

Peer Global File Service รวมถึงความสามารถในการผลักดันข้อมูลจาก NAS devices ไปยัง object storage เทคโนโลยี real-time replication เดียวกันกับที่ใช้ใน collaboration scenario ด้านบนสามารถถูกนำมาใช้เพื่อ replicate ข้อมูลไปยัง Nutanix Objects ได้เช่นกัน พร้อมรองรับความสามารถแบบ optional snapshot สำหรับ point-in-time recovery โดย objects ทั้งหมดจะถูก replicate ในรูปแบบที่โปร่งใส (transparent format) ซึ่งสามารถนำไปใช้โดยแอปพลิเคชันและบริการอื่นได้ทันที

ส่วนนี้ของแล็บจะแนะนำคุณผ่านขั้นตอนที่จำเป็นในการ replicate ข้อมูลจาก Nutanix Files ไปยัง Nutanix Objects

### Getting Client IP and Credentials for Nutanix Objects

ในการ replicate ข้อมูลไปยัง Objects คุณจำเป็นต้องมี Client IP ของ object store และต้องสร้าง access และ secret keys หากคุณมีข้อมูลนี้จากแล็บก่อนหน้าอยู่แล้ว คุณสามารถข้ามส่วนนี้และใช้ข้อมูลเดิมซ้ำได้

1.  ล็อกอินเข้าสู่ **Prism Central** (เช่น 10.XX.YY.39) บน Nutanix cluster ของคุณ จากนั้นไปที่ **Services** > **Objects**.
    
2.  ในส่วน **Object Stores**, ค้นหา object store ที่เหมาะสมในตารางและจดบันทึก Client Used IPs
    
    ![](/images/clientip.da498452.png)
    
3.  คลิกที่ส่วน **Access Keys** และคลิก **Add People** เพื่อเริ่มกระบวนการสร้าง credentials
    
    ![](/images/buckets_add_people.ac751801.png)
    
4.  เลือก **Add people not in Active Directory** และกรอกที่อยู่อีเมลของคุณ
    
    ![](/images/buckets_add_people2.d2a7c619.png)
    
5.  คลิก **Next**.
    
6.  คลิก **Download Keys** เพื่อดาวน์โหลดไฟล์ .csv ซึ่งประกอบด้วย **Access Key** และ **Secret Key**.
    
    ![](/images/buckets_add_people3.d7cdd84b.png)
    
7.  คลิก **Close**.
    
8.  เปิดไฟล์ด้วย text editor
    
    ![](/images/buckets_csv_file.848ff96b.png)
    
    !!! note
        เปิดไฟล์ text ทิ้งไว้เพื่อให้คุณมี access และ secret keys พร้อมใช้งานสำหรับส่วนด้านล่าง
    

### Creating a New Cloud Replication Job

ในส่วนนี้ เราจะมุ่งเน้นไปที่การสร้าง job ประเภท **Cloud Backup and Replication** เพื่อ replicate ข้อมูลจาก Nutanix Files ไปยัง Nutanix Objects

1.  ใน **PMC Web Interface**, คลิก **File > New Job**.
    
    ![](/images/cloud1.7bc0ad26.png)
    
2.  เลือก **Cloud Backup and Replication** แล้วคลิก **Create**.
    
3.  ป้อน `User##`**\-Replication to Objects** เป็นชื่อสำหรับ job แล้วคลิก **OK**.
    
    ![](/images/cloud2.9b3dd8b7.png)
    
4.  เลือก **Nutanix Files** แล้วคลิก **Next**.
    
    ![](/images/cloud3.909257b7.png)
    
5.  เลือก Agent ที่ชื่อ **PeerAgent-Files** แล้วคลิก **Next**. Agent นี้จะทำหน้าที่จัดการ Files cluster
    
    ![](/images/cloud4.17399f25.png)
    
6.  บนหน้า **Storage Information**, คุณจะเห็น 1 ใน 2 หน้านี้ หากผู้เข้าร่วมคนอื่นที่แชร์ Files cluster ของคุณได้ทำแล็บ Peer ไปแล้ว คุณสามารถเลือก **Existing Credentials** ของพวกเขาได้ตามที่แสดงที่นี่
    
    ![](/images/cloud5.b60a22c0.png)
    
    หากคุณเป็นผู้เข้าร่วมคนแรกบน cluster นี้ที่ทำแล็บ Peer ให้กรอกข้อมูลในฟิลด์ต่อไปนี้:
    
    -   **Nutanix Files Cluster Name** - **BootcampFS**
        
        ชื่อ NETBIOS ของ Files cluster ที่จะจับคู่กับ Agent ที่เลือกในขั้นตอนก่อนหน้า
        
    -   **Username** - `peer`
        
        นี่คือ username ของบัญชี Files API ที่ถูกตั้งค่าไว้ก่อนหน้านี้ในแล็บ และต้องเป็นตัวพิมพ์เล็กทั้งหมด
        
    -   **Password** - `nutanix/4u`
        
        รหัสผ่านที่เกี่ยวข้องกับบัญชี Files API
        
    -   **Peer Agent IP** - **PeerAgent-Files** IP Address
        
        IP address ของ Agent server ที่จะรับ real-time notifications จาก File Activity Monitoring API ที่มาพร้อมกับ Files โดยสามารถเลือกได้จาก dropdown list ของ IPs ที่ใช้งานได้บน Agent server นี้
        
7.  คลิก **Validate** เพื่อยืนยันว่า Files สามารถเข้าถึงได้ผ่าน API โดยใช้ credentials ที่ให้ไว้
    
    ![](/images/cloud6.86c46dee.png)
    
    !!! note    
        เมื่อคุณป้อน credentials เหล่านี้ พวกมันจะถูกนำมาใช้ซ้ำได้เมื่อสร้าง jobs ใหม่ที่ใช้ Agent เฉพาะเจาะจงตัวนี้ เมื่อคุณสร้าง job ถัดไป ให้เลือก **Existing Credentials** ในหน้านี้เพื่อแสดงรายการ credentials ที่เคยถูกกำหนดค่าไว้แล้ว
    
8.  คลิก **Next**.
    
9.  เลือก `User##`**\-Peer** share ของคุณแล้วคลิก **OK**.
    
    ![](/images/cloud7.e2e0a894.png)
    
    !!! note
        Peer Global File Service รองรับการ replication ของข้อมูลภายใน nested shares โดยเริ่มตั้งแต่ Nutanix Files v3.5.1 ขึ้นไป
    
    ด้วย **Cloud Backup and Replication**, คุณสามารถเลือก multiple shares และ/หรือ folders สำหรับ single job ได้
    
10.  ในหน้า **File Filters**, ให้ตรวจสอบตัวกรอง **Default** ที่ถูกเลือกไว้รวมถึง **Include Files Without Extensions**, แล้วคลิก **Next**.
    
    ![](/images/cloud8.1e7ae167.png)
    
11.  ในหน้า **Destination**, เลือก **Nutanix Objects** แล้วคลิก **Next**.
    
    ![](/images/cloud9.ea12c80a.png)
    
12.  บนหน้า **Nutanix Objects Credentials**, กรอกฟิลด์ต่อไปนี้:
    
    -   **Description** -- ตั้งชื่อ destination ของคุณ
        
        นี่คือชื่อย่อ (short name) สำหรับการกำหนดค่า Objects credential
        
    -   **Access Key**
        
        Access Key ที่เชื่อมโยงกับบัญชี Objects
        
    -   **Secret Key**
        
        Secret Key ที่เชื่อมโยงกับบัญชี Objects
        
    -   **Service Point**
        
        client access IP address หรือ FDQN name ของ object store
        
    
    ![](/images/cloud10.b961d7ad.png)
    
    !!! note    
        ดูอ้างอิงในส่วน [Getting Client IP and Credentials for Nutanix Objects](#getting-client-ip-and-credentials-for-nutanix-objects) ด้านบนสำหรับ access และ secret keys ที่เหมาะสม รวมถึง Client IP ของ object store
    
13.  คลิก **Validate** เพื่อยืนยันว่า Objects สามารถเข้าถึงได้โดยใช้ configuration ที่ให้ไว้
    
    ![](/images/cloud11.dadb2892.png)
    
14.  คลิก **OK** ในหน้าต่าง **Success**, จากนั้นคลิก **Next**.
    
15.  บนหน้า **Bucket Details**, ยกเลิกการเลือกช่อง **Automatically name**, และจากนั้นระบุชื่อ bucket ที่ไม่ซ้ำกัน คือ `User##`**\-peer-objects**.
    
    ![](/images/cloud12.6598df7c.png)
    
    !!! note    
        ชื่อ bucket ต้องเป็นตัวพิมพ์เล็กทั้งหมด
    
16.  บนหน้า **Replication and Retention Policy**, เลือก **Existing Policy**, **Continuous Data Protection**, แล้วคลิก **Next**.
    
    ![](/images/cloud13.1e1ee49d.png)
    
17.  คลิก **Next** ในหน้า **Miscellaneous Options**, **Email Alerts**, และ **SNMP Alerts**.
    
18.  ตรวจสอบการตั้งค่าในหน้า **Confirmation**, แล้วคลิก **Finish**.
    
    ![](/images/cloud14.819316b8.png)
    

### Starting a Cloud Replication Job

เมื่อสร้าง job แล้ว จะต้องถูก start เพื่อเริ่มต้น replication

1.  ใน **PMC Web Interface**, ให้คลิกขวาที่ job ที่เพิ่งสร้างใหม่ของคุณ แล้วเลือก **Start**.
    
    ![](/images/cloud15.67e6b0da.png)
    
2.  ดับเบิลคลิก job ใน pane **Job** เพื่อดู runtime information และ statistics ของมัน
    
    ![](/images/cloud16.2a6d2191.png)
    
    !!! note    
        คลิก **Auto-Update** เพื่อให้ console รีเฟรชตัวเองอย่างสม่ำเสมอเมื่อไฟล์เริ่มทำการ replicate
    

### Verifying Replication

วิธีที่ง่ายที่สุดในการตรวจสอบว่าไฟล์ถูก replicate ไปยัง Nutanix Objects เรียบร้อยแล้วคือการใช้เครื่องมือ Cyberduck บน VM `User##`\-WinTools ของคุณ

1.  เชื่อมต่อไปยัง `User##`\-WinTools VM ของคุณผ่าน RDP โดยใช้ credentials ต่อไปนี้:
    
    -   **Username** - `NTNXLAB\Administrator`
    -   **Password** - `nutanix/4u`

2.  เปิดโปรแกรม **Cyberduck** (คลิกไอคอน Window > ลูกศรชี้ลง > Cyberduck).
    
    หากคุณได้รับ prompt ให้ทำการอัปเดต Cyberduck ให้คลิก **Skip This Version**.
    
3.  คลิกที่ **Open Connection**.
    
    ![](/images/buckets_06.3fc32617.png)
    
4.  เลือก **Amazon S3** จากรายการ dropdown
    
    ![](/images/buckets_07.16aa1972.png)
    
5.  กรอกข้อมูลในฟิลด์ต่อไปนี้สำหรับผู้ใช้ที่สร้างไว้ก่อนหน้านี้ แล้วคลิก **Connect**:
    
    -   **Server** - _Objects Client Used IP_
    -   **Port** - 443
    -   **Access Key ID** - _ถูกสร้างตอนที่ User ถูกสร้าง_
    -   **Password (Secret Key)** - _ถูกสร้างตอนที่ User ถูกสร้าง_
    
    !!! note
        ดูส่วน [Getting Client IP and Credentials for Nutanix Objects](#getting-client-ip-and-credentials-for-nutanix-objects) ด้านบนสำหรับ access และ secret keys ที่เหมาะสม รวมถึง Client IP ของ object store
    
    ![](/images/buckets_08.61cd8a74.png)
    
6.  ทำเครื่องหมายที่ช่อง **Always Trust**, จากนั้นคลิก **Continue** ในกล่องข้อความ **The certificate is not valid**.
    
    ![](/images/invalid_certificate.4ff0bc5d.png)
    
7.  คลิก **Yes** เพื่อทำการติดตั้ง self-signed certificate ต่อไป
    
8.  ไปยัง bucket ที่ตั้งค่าไว้ด้านบนและตรวจสอบว่ามีเนื้อหาอยู่ภายใน
    
    ![](/images/cloud19.045da0a3.png)
    

ขอแสดงความยินดี! คุณประสบความสำเร็จในการตั้งค่า replication ระหว่าง Nutanix Files และ Nutanix Objects! การใช้ Peer ทำให้แนวทางเดียวกันนี้สามารถนำไปใช้เพื่อสนับสนุน scenarios ต่างๆ ซึ่งรวมถึงการใช้งานข้อมูลไฟล์ร่วมกับ object-based apps และบริการ (services) ควบคู่ไปกับการทำ point-in-time recovery ของข้อมูล enterprise NAS ที่สำรองด้วย Objects

## Analyzing Existing Environments

ในขณะที่ capacity ของสภาพแวดล้อม file server เพิ่มขึ้นในอัตราที่รวดเร็วเป็นประวัติการณ์, storage admins มักจะไม่ทราบว่า users และ applications นำสภาพแวดล้อม file server เหล่านี้ไปใช้งานอย่างไร ข้อเท็จจริงนี้จะเห็นได้ชัดเจนที่สุดเมื่อถึงเวลาที่จะ migrate ไปยัง storage platform ใหม่ File System Analyzer คือเครื่องมือจาก Peer Software ที่ออกแบบมาเพื่อช่วย partners ค้นหาและวิเคราะห์โครงสร้างไฟล์และโฟลเดอร์ที่มีอยู่ เพื่อวัตถุประสงค์ในการวางแผน (planning) และการเพิ่มประสิทธิภาพ (optimization)

File System Analyzer ดำเนินการแสกนเส้นทาง (paths) ที่กำหนดหนึ่งเส้นทางหรือมากกว่านั้นอย่างรวดเร็วมาก, ทำการ upload ผลลัพธ์ไปยัง Amazon S3, ประกอบ key pieces ของข้อมูลลงใน Excel workbooks เล่มเดียวหรือหลายเล่ม, และอีเมลรายงานพร้อม links เพื่อเข้าถึง workbooks ดังกล่าว

### Installing and Running the File System Analyzer

1.  เชื่อมต่อไปยัง `User##`\-WinTools VM ของคุณผ่าน RDP โดยใช้ credentials ต่อไปนี้:
    
    -   **Username** - `NTNXLAB\Administrator`
    -   **Password** - `nutanix/4u`

2.  ภายใน VM ให้ดาวน์โหลดตัวติดตั้ง File System Analyzer: [https://www.peersoftware.com/downloads/fsa/13/FileSystemAnalyzer\_win64.exe](https://www.peersoftware.com/downloads/fsa/13/FileSystemAnalyzer_win64.exe)
    
3.  เรียกใช้ตัวติดตั้งและเลือก **Immediate Installation**.
    
    ![](/images/fsa1.08ca59bb.png)
    
    เมื่อการติดตั้งเสร็จสิ้น ตัวช่วยสร้าง (wizard) File System Analyzer จะถูกเปิดขึ้นโดยอัตโนมัติ
    
4.  หน้าจอ **Introduction** จะให้รายละเอียดเกี่ยวกับข้อมูลที่ถูกรวบรวมและรายงานโดย utility นี้ คลิก **Next**.
    
    ![](/images/fsa2.3869d694.png)
    
5.  หน้าจอ **Contact Information** จะรวบรวมข้อมูลที่ใช้เพื่อจัดระเบียบ output ของ File System Analyzer และเพื่อส่ง final reports กรอกข้อมูลในฟิลด์ต่อไปนี้:
    
    -   **Company** -- ป้อนชื่อบริษัทของคุณ
    -   **Location** -- ป้อนสถานที่ตั้ง (physical location) ของเซิร์ฟเวอร์ที่กำลังรัน File System Analyzer ในสภาพแวดล้อมแบบ multi-site นี่อาจเป็นชื่อเมืองหรือรัฐ หรือใช้ชื่อ data center ก็ได้
    -   **Project** -- ป้อนชื่อโปรเจกต์หรือเหตุผลทางธุรกิจ (business reason) สำหรับการรันการวิเคราะห์นี้ ข้อมูลนี้ (และฟิลด์ Company รวมถึง Location) จะถูกใช้เพื่อจัดระเบียบ final reports เท่านั้น
    -   **Mode** -- เลือก mode การทำงานที่จะถูกนำมาใช้ -- **General Analysis** หรือ **Migration Preparation**. โหมด **Migration Preparation** จะเป็นประโยชน์เมื่อต้องการเตรียมความพร้อมสำหรับ migration project ระหว่าง storage systems นอกเหนือจากการรวบรวม standard telemetry บน file systems แล้ว โหมดนี้ยังมีตัวเลือกในการทดสอบประสิทธิภาพ (performance) ของทั้งระบบ storage เก่าและใหม่ เพื่อช่วยประเมิน migration performance และเวลา (timing) ที่อาจเกิดขึ้น สำหรับแล็บนี้ เราจะใช้ **General Analysis**.
    -   **Name/Phone/Title** -- _(Optional)_ ป้อนชื่อและข้อมูลการติดต่อของคุณ
    -   **Email** -- ป้อนที่อยู่อีเมลที่จะรับ final reports สำหรับการป้อนหลายที่อยู่ ให้คั่นด้วยเครื่องหมายจุลภาค (comma)
    -   **Upload Region** -- เลือก **US**, **EU**, หรือ **APAC** เพื่อบอกให้ File System Analyzer ทราบว่าจะใช้ S3 location ไหนสำหรับการ upload final reports
    
    !!! note    
        อย่าลืมป้อนรายละเอียดของคุณเองลงในหน้า wizard ที่แสดงด้านล่าง ไม่เช่นนั้น final report จะไม่ถูกส่งไปให้คุณ
    
    ![](/images/fsa3.9e98b84e.png)
    
6.  คลิก **Next**.
    
    File System Analyzer สามารถกำหนดค่าให้แสกนเส้นทาง (paths) เดียวหรือหลายเส้นทางได้ โดย paths เหล่านี้สามารถเป็นแบบ local (เช่น `D:\MyData`) หรือ Remote UNC Path (เช่น `\files01\homes1`).
    
7.  เพิ่ม paths ต่อไปนี้:
    
    -   `C:\` - ไดรฟ์ C: local ของ `User##`\-WinTools VM
    -   `\BootcampFS\<Your Share Name>\` - share ที่ถูกสร้างไว้ก่อนหน้านี้บน Nutanix Files
    
    ![คลิกปุ่ม Search และป้อนชื่อของ file server หากคุณต้องการค้นหา shares ที่มีอยู่บน file server นั้น คุณยังสามารถคลิกขวาภายใน dialog และเลือก Check All เพื่อเพิ่ม shares ที่ค้นพบทั้งหมดโดยอัตโนมัติ](/images/fsa4.7772f6be.png)
    
    ![การเลือกตัวเลือก Log totals by owner จะตรวจสอบทุกไฟล์และโฟลเดอร์ภายใน scan path(s) ที่เลือกเพื่อหาเจ้าของ ข้อมูลเจ้าของนี้จะถูกนับโดยแยกเป็น bytes, files, และ folders และรวมอยู่ใน final report](/images/fsa4a.5309794d.png)
    
8.  คลิก **Next**.
    
    คลิกปุ่ม **Start** เพื่อเริ่มการแสกนเส้นทาง (paths) ที่ป้อน เมื่อกระบวนการ scans, analyses, และ uploads เสร็จสมบูรณ์ทั้งหมด คุณจะเห็นสถานะที่คล้ายกับด้านล่างนี้:
    
    ![](/images/fsa5.48a6bb12.png)
    
9.  File System Analyzer จะทำการอีเมลรายงานไปยังที่อยู่ (addresses) ทั้งหมดที่ตั้งค่าไว้ ในการดู full report, ให้คลิก hyperlink(s) ที่อยู่ใต้ **Detailed Reports** ในอีเมล หากมีหลาย paths ถูกแสกน คุณจะเห็น link ไปยัง cumulative report ซึ่งครอบคลุมทุก paths ด้วย
    
    ![](/images/fsa6.8d5edfaa.png)
    
    !!! note    
        Links สำหรับการดาวน์โหลดรายงานจะเปิดใช้งานเป็นเวลา **24 ชั่วโมง** เท่านั้น ติดต่อ Peer Software หากต้องการเข้าถึงรายงานที่หมดอายุแล้ว
    
    บางระบบอาจเปิด workbooks เหล่านี้ในโหมดที่ได้รับการป้องกัน (protected mode) โดยจะแสดงข้อความนี้ใน Excel:
    
    ![](/images/fsa8.73acbf48.png)
    
    หากคุณเห็นข้อความนี้ที่ด้านบนของ Excel ให้คลิก **Enable Editing** เพื่อเปิด workbook อย่างสมบูรณ์ หากคุณไม่ทำเช่นนี้ pivot tables และ charts จะไม่โหลดอย่างถูกต้อง
    

### [#](#summary-reports) Summary Reports

Summary reports จะประกอบด้วยข้อมูลทางสถิติและประวัติแบบภาพรวม (overall statistical and historical information) ข้าม paths ทั้งหมดที่ถูกเลือกเพื่อแสกน เมื่อคุณเปิด summary report ขึ้นมา คุณจะพบกับ worksheet ที่มีลักษณะแบบนี้:

![](/images/fsa7.8741be91.png)

แต่ละ summary report อาจประกอบด้วย worksheets ต่อไปนี้ทั้งหมดหรือบางส่วน:

-   **InfoSheet** -- รายละเอียดเกี่ยวกับการรัน (run) ครั้งนี้ หน้านี้จะแสดง Total Bytes ในรูปแบบฐานสิบ (1 KB เท่ากับ 1,000 bytes) และฐานสอง (1 KiB เท่ากับ 1,024 bytes) ด้วย
-   **CollectiveResults** -- รายการของ paths ทั้งหมดที่ถูกแสกนพร้อมกับสถิติระดับสูง (high-level statistics) สำหรับแต่ละ path
-   **History-Bytes** -- ประกอบด้วยการเปลี่ยนแปลงในอดีต (historical changes) ในรูปแบบ bytes ในแต่ละครั้งที่มีการแสกนแต่ละ path
-   **History-Files** -- ประกอบด้วยการเปลี่ยนแปลงในอดีตสำหรับจำนวนรวมของไฟล์ (total number of files) ในแต่ละครั้งที่มีการแสกนแต่ละ path
-   **History-Folders** -- ประกอบด้วยการเปลี่ยนแปลงในอดีตสำหรับจำนวนรวมของโฟลเดอร์ (total numbers of folders) ในแต่ละครั้งที่มีการแสกนแต่ละ path

!!! note
    History worksheets จะปรากฏขึ้นหลังจากที่มีการรัน multiple scans แล้วเท่านั้น

### [#](#volume-reports) Volume Reports

Volume reports จะให้ข้อมูลที่ละเอียดกว่าสำหรับ specific path ที่ถูกแสกน เมื่อคุณเปิด volume report ขึ้นมา คุณจะพบกับ worksheet ที่มีลักษณะแบบนี้:

![](/images/fsa7a.d03643f8.png)

แต่ละ volume report อาจประกอบด้วย worksheets ต่อไปนี้ทั้งหมดหรือบางส่วน:

-   **Overview** -- ชุดของ pivot tables และ charts ที่แสดงสถิติระดับสูง (high-level statistics) เกี่ยวกับ path ที่ถูกแสกน
-   **InfoSheet** -- รายละเอียดเกี่ยวกับการสแกน (scan) ครั้งนี้ หน้านี้จะแสดง Total Bytes ในรูปแบบฐานสิบและฐานสอง
-   **OverallStats** -- สถิติโดยรวมของโฟลเดอร์ที่ถูกแสกน ซึ่งประกอบไปด้วย total bytes, files, folders ฯลฯ
-   **Analysis** -- ประกอบด้วย pivot table และคู่ของ charts ที่เน้นสถิติเพิ่มเติมเกี่ยวกับ path ที่ถูกแสกน
-   **History** -- แสดงสถิติจากแต่ละครั้งของการสแกน volume นี้
-   **HistoryCharts** -- ประกอบด้วย charts ที่แสดงการเปลี่ยนแปลงในอดีต (historical changes) ของ files, folders, และ bytes สำหรับ volume นี้
-   **HighSubFolderCounts** -- รายการโฟลเดอร์ทั้งหมดที่มี child directories มากกว่า 100 รายการ
-   **HighByteCounts** -- รายการโฟลเดอร์ทั้งหมดที่มี child file data มากกว่า 10GB
-   **HighFileCounts** -- รายการโฟลเดอร์ทั้งหมดที่มี child files มากกว่า 10,000 ไฟล์
-   **LargeFiles** -- รายการของไฟล์ทั้งหมดที่ตรวจพบว่ามีขนาด 10GB หรือใหญ่กว่า
-   **DeepPaths** -- รายการของ folder paths ทั้งหมดที่ตรวจพบว่าลึก 15 ระดับหรือลึกกว่า
-   **LongPaths** -- รายการของ folder paths ทั้งหมดที่ตรวจพบว่ายาว 256 ตัวอักษรหรือยาวกว่า
-   **ReparsePointsSummary** -- สรุปของ reparse points ทั้งหมดที่ตรวจพบ โดยไม่สนว่าจะเป็นไฟล์หรือโฟลเดอร์
-   **ReparsePoints** -- รายการของ folder reparse points ทั้งหมดที่ตรวจพบ
-   **TimeAnalysis** -- การแจกแจงแยกรายละเอียดของ total files, folders, และ bytes ตามอายุการสร้าง (age)
-   **LastModifiedAnalysis** -- มุมมองของ files, folders, และ bytes ทั้งหมดที่ถูกแก้ไขในแต่ละชั่วโมงตลอดหนึ่งปีที่ผ่านมา ตัวเลขเหล่านี้จะถูกนำมาคำนวณยอดรวม (totaled) และค่าเฉลี่ย (averaged) เพื่อแสดงจำนวน files, folders, และ bytes ที่ถูกแก้ไข โดยแยกตาม: วันในสัปดาห์ (day of week); เดือน (month); ชั่วโมงในแต่ละวัน (hour of the day); วันที่ของเดือน (day of month); และวันของปี (day of year)
-   **CreatedAnalysis** -- มุมมองของ files, folders, และ bytes ทั้งหมดที่ถูกสร้างในแต่ละชั่วโมงตลอดหนึ่งปีที่ผ่านมา ตัวเลขเหล่านี้จะถูกนำมาคำนวณยอดรวมและค่าเฉลี่ยเพื่อแสดงจำนวน files, folders, และ bytes ที่ถูกสร้าง โดยแยกตาม: วันในสัปดาห์, เดือน, ชั่วโมงในแต่ละวัน, วันที่ของเดือน, และวันของปี
-   **LastAccessedAnalysis** -- มุมมองของ files, folders, และ bytes ทั้งหมดที่ถูกเข้าถึง (accessed) ในแต่ละชั่วโมงตลอดหนึ่งปีที่ผ่านมา ตัวเลขเหล่านี้จะถูกนำมาคำนวณยอดรวมและค่าเฉลี่ยเพื่อแสดงจำนวน files, folders, และ bytes ที่ถูกเข้าถึง โดยแยกตาม: วันในสัปดาห์, เดือน, ชั่วโมงในแต่ละวัน, วันที่ของเดือน, และวันของปี
-   **TLDAnalysis** - รายการของแต่ละโฟลเดอร์ที่อยู่ภายใต้ path ที่กำหนดโดยตรง (immediately) พร้อมด้วยสถิติสำหรับแต่ละ subfolders เหล่านี้ ในสภาพแวดล้อมที่เป็น user home directory โฟลเดอร์ย่อย (subfolders) แต่ละรายการควรจะสื่อถึง user ที่แตกต่างกัน
-   **TopTLDsByTotals** -- ชุดของ pivot tables และ charts แสดง top-level directories จำนวน 10 อันดับแรก อ้างอิงตาม total bytes used, total files, และ total folders
-   **TopTLDsByLastModBytes** -- pivot table และ chart แสดง top-level directories จำนวน 10 อันดับแรก อ้างอิงตามจำนวน bytes ส่วนใหญ่ที่ถูกแก้ไขในรอบปีที่ผ่านมา
-   **TopTLDsByLastModFiles** -- pivot table และ chart แสดง top-level directories จำนวน 10 อันดับแรก อ้างอิงตามจำนวนไฟล์ส่วนใหญ่ที่ถูกแก้ไขในรอบปีที่ผ่านมา
-   **LegacyTLDs** -- รายการของ top-level directories ทั้งหมดที่ไม่มีไฟล์ใดถูกแก้ไขในช่วง 365 วันที่ผ่านมา
-   **TreeDepth** -- การนับคะแนนรวม (tally) ของ bytes, folders, และ files ที่พบในแต่ละระดับความลึก (depth level) ของโครงสร้างโฟลเดอร์ สำหรับลูกค้าที่ทำการวิเคราะห์แบบ pre-migration ค่าความลึกที่แสดงเป็นสีเขียวเป็น candidates ที่ดีสำหรับการตั้งค่า tree depth ของ PeerSync Migration
-   **FileExtInfo** -- รายการ extensions ทั้งหมดที่ตรวจพบ รวมถึง pivot tables ที่เรียงตาม total bytes และ total files
-   **FileAttributes** -- สรุปของคุณลักษณะไฟล์และโฟลเดอร์ (file and folder attributes) ทั้งหมดที่พบ
-   **SmallFileAnalysis** -- รายการของไฟล์ทั้งหมดที่ตรวจพบซึ่งมีขนาดต่ำกว่าขนาดที่กำหนด หน้านี้มีประโยชน์สำหรับการประเมินผลกระทบต่อ storage (storage impact) ของไฟล์ขนาดเล็ก บน storage platforms ที่มีการกำหนดขนาดไฟล์ขั้นต่ำ (minimum file sizes) บนดิสก์ขนาดใหญ่
-   **SIDCache** -- รายการของเจ้าของ (owners) และ SID strings ทั้งหมดที่ตรวจพบ

!!! note
    History worksheets จะปรากฏขึ้นหลังจากที่มีการรัน multiple scans แล้วเท่านั้น

นี่คือตัวอย่างของหน้า **LastModifiedAnalysis** ที่กล่าวถึงด้านบน:

![](/images/fsa7b.32b46f08.png)

## Integrating with Microsoft DFS Namespace

Peer Global File Service รวมถึงความสามารถในการสร้างและจัดการ Microsoft DFS Namespaces (DFS-N) เมื่อการบูรณาการ (integration) DFS-N นี้ถูกนำไปรวมกับ real-time replication และ file locking engine ของมัน PeerGFS จะสามารถขับเคลื่อนระบบ global namespace อย่างแท้จริงที่ครอบคลุม (spans) locations และ storage devices ต่างๆ

ในฐานะส่วนหนึ่งของความสามารถการจัดการ DFS namespace, PeerGFS ยังทำการเปลี่ยนเส้นทาง (redirects) ของผู้ใช้ให้ออกห่างจาก file server ที่มีปัญหา (failed file server) โดยอัตโนมัติ เมื่อเซิร์ฟเวอร์ที่มีปัญหานั้นกลับมาทำงานปกติ (online), PeerGFS จะนำ file server ตัวนี้กลับเข้าสู่สถานะ in-sync จากนั้นจึงเปิดใช้งานให้ผู้ใช้สามารถเข้าถึงได้อีกครั้ง *นี่คือคุณสมบัติ Disaster Recovery ที่จำเป็นสำหรับ deployment ใดๆ ที่ต้องการใช้ประโยชน์จาก Nutanix Files สำหรับการแชร์ user profile และ user data ภายในสภาพแวดล้อม VDI

ภาพหน้าจอ (screenshot) ต่อไปนี้แสดงถึง PMC interface ซึ่งมี DFS Namespace ที่อยู่ภายใต้การจัดการ

![](/images/dfsn.43c6ff26.png)

## Takeaways

-   Peer Global File Service คือ solution เพียงหนึ่งเดียวที่สามารถจัดหา Active-Active replication สำหรับ Nutanix Files clusters ได้
-   Peer ยังรองรับ legacy NAS platforms หลายแพลตฟอร์ม และรองรับการ replication ภายในสภาพแวดล้อมที่ผสมผสาน (mixed environments) สิ่งนี้ช่วยทำให้การปรับใช้และการ migration ไปยัง Nutanix Files เป็นเรื่องง่ายขึ้น
-   Peer สามารถจัดการ Microsoft Distributed File Services (DFS) namespaces ได้โดยตรง ทำให้ multiple file servers ถูกนำเสนอ (presented) ผ่าน single namespace ได้ นี่คือส่วนประกอบสำคัญ (key component) สำหรับการสนับสนุนระบบ Active-Active DR solutions ที่แท้จริงสำหรับการทำ file sharing
-   Peer สามารถทำการ replicate ไฟล์จาก Nutanix Files และ NAS platforms อื่นๆ ไปยัง Nutanix Objects พร้อมความสามารถด้าน optional snapshot สำหรับ point-in-time recovery Objects ทั้งหมดจะอยู่ในรูปแบบที่โปร่งใส ซึ่งแอปพลิเคชันและบริการอื่นสามารถนำไปใช้งานได้ทันที
-   Peer มีเครื่องมือสำหรับการวิเคราะห์ file servers ที่มีอยู่เพื่อช่วยเหลือด้านการวางแผนทรัพยากร (resource planning), การเพิ่มประสิทธิภาพ (optimization), และทำให้เกิด migrations โดยส่งผลกระทบให้เกิดการหยุดชะงักน้อยที่สุด (minimally disruptive)
