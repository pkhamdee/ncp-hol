# Monitoring VMs with Reports

## Overview

ฟีเจอร์ report management ช่วยให้คุณสามารถ configure และจัดส่ง historical reports ที่ประกอบด้วยข้อมูลโดยละเอียดเกี่ยวกับ infrastructure resources ทั่วทั้ง Nutanix-managed และ non-Nutanix-managed environments เครื่องมือนี้ให้ operational insights ที่มีค่าส่งตรงไปยัง mailbox ของคุณตาม schedule ที่คุณกำหนด

**Key Capabilities**

-   การสร้าง (Generation) PDF และ CSV Report
-   การทำ Report Scheduling
-   Predefined Out-of-the-Box Reports
-   การทำ Import และ Export Reporting Configuration ข้าม PCs

![](/images/report6.aa64f8ea.png)

## Create a Report

IT Administrator ต้องการข้อมูลเกี่ยวกับ production infrastructure ของพวกเขาในช่วง 6 เดือนที่ผ่านมา เพื่อให้ได้รับ insights เกี่ยวกับ utilization efficiency มาดูวิธีที่จะนำ reports ไปใช้เพื่อจัดการกับ scenario นี้กัน

1.  Login เข้าสู่ Prism Central โดยใช้ `adminuser##` และ PC password จากหน้า Connection Details
    
2.  ไปที่ส่วน App Switcher ที่มุมซ้ายบนของ Prism Central คลิก **Intelligent Operations** ใน App Switcher
    
3.  เลือก **Reports** จาก Intelligent Operations Dashboard
    
4.  เราสามารถดูรายการ pre-defined system reports ตลอดจน custom reports ได้

    ![](/images/report1.61a9a0d1.png)

5.  เราจะค้นหา Cluster Usage report ตาม use case ของเรา ป้อน "**Cluster Usage**" ในฟิลด์ filter by

    ![](/images/report2.2d08c57d.png)

6.  เลือก **Cluster Usage Summary** และคลิก **Actions**

    ![](/images/report3.c888522c.png)

7.  report นี้เป็น pre-defined system Report ตัว System report สามารถถูก run, scheduled, shared และ cloned เพื่อทำการเปลี่ยนแปลงได้ ตอนนี้ให้เลือก **Run**
    
8.  ระบุชื่อในฟิลด์ **Report Instance Name**
    
9.  เลือก cluster ในฟิลด์ **Entity**
    
10.  เลือก **Last 24 Hours** ใน Time Period for Report
    
11.  เลือก Time Zone ของคุณ
    
12.  เลือก PDF เป็น Report Format
    
    ![](/images/report4.68cbb778.png)

13.  ป้อน Email Address ของคุณ หากคุณต้องการให้ส่ง report ไปให้คุณทางอีเมล
    
14.  คลิก **Run**
    
15.  คลิก **Generated Reports** เพื่อดู Report
    
16.  คลิก PDF ถัดจากชื่อ Cluster Usage Summary Report Name
    
17.  Report จะถูกดาวน์โหลดในรูปแบบ PDF
    
18.  นี่คือตัวอย่างลักษณะของ report ที่จะได้
    
    ![](/images/report5.43db3977.png)
