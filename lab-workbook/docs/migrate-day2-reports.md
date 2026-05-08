# Monitoring VMs with Reports

## Overview

ฟีเจอร์ report management ช่วยให้คุณสามารถ configure และจัดส่ง historical reports ที่มีข้อมูลโดยละเอียดเกี่ยวกับ infrastructure resources ทั้งใน environments แบบ Nutanix-managed และ non-Nutanix-managed ได้ เครื่องมือนี้จะให้ operational insights ที่มีค่าส่งตรงไปยัง mailbox ของคุณตาม schedule ที่คุณกำหนด

**Key Capabilities**

-   การสร้าง Report แบบ PDF และ CSV
-   การทำ Report Scheduling
-   18 Predefined Out-of-the-Box Reports ที่มีมาให้พร้อมใช้งาน
-   การทำ Import และ Export สำหรับ Reporting Configuration ข้ามไปยัง PCs ต่างๆ

![](/images/report6.aa64f8ea.png)

## Create a Report

IT Administrator ต้องการข้อมูลเกี่ยวกับ production infrastructure ในช่วง 6 เดือนที่ผ่านมา เพื่อดู insights เกี่ยวกับ utilization efficiency (ประสิทธิภาพการใช้งาน) มาดูกันว่าเราจะสามารถใช้ reports เพื่อจัดการกับ scenario นี้ได้อย่างไร

1.  Login เข้าสู่ Prism Central โดยใช้ `adminuser##` และ PC password จากหน้า Connection Details
    
2.  ไปที่ส่วนของ App Switcher ที่มุมบนซ้ายของ Prism Central คลิก **Intelligent Operations** ใน App Switcher
    
3.  เลือก **Reports** จาก Intelligent Operations Dashboard
    
4.  เราสามารถดูรายการของ pre-defined system reports รวมถึง custom reports ได้
    
    ![](/images/report1.61a9a0d1.png)

5.  เราจะค้นหา Cluster Usage report ตาม use case ของเรา ให้พิมพ์ "**Cluster Usage**" ลงในฟิลด์ filter by

    ![](/images/report2.2d08c57d.png)

6.  เลือก **Cluster Usage Summary** และคลิก **Actions**

    ![](/images/report3.c888522c.png)

7.  report นี้คือ pre-defined system Report โดย System report จะสามารถทำการ run, scheduled, shared และ cloned เพื่อแก้ไขเปลี่ยนแปลงได้ ตอนนี้ ให้เลือก **Run**
    
8.  ระบุชื่อลงในฟิลด์ **Report Instance Name**
    
9.  เลือก cluster ในฟิลด์ **Entity**
    
10.  เลือก **Last 24 Hours** ใน Time Period สำหรับ Report
    
11.  เลือก Time Zone ของคุณ
    
12.  เลือก PDF เป็น Report Format
    
    ![](/images/report4.68cbb778.png)

13.  ระบุ Email Address ของคุณ หากคุณต้องการให้ส่ง report ไปยังอีเมลของคุณ
    
14.  คลิก **Run**
    
15.  คลิก **Generated Reports** เพื่อดู Report
    
16.  คลิก PDF ถัดจากชื่อ Cluster Usage Summary Report
    
17.  ตัว Report จะถูก downloaded ในรูปแบบ PDF
    
18.  นี่คือตัวอย่างหน้าตาของ report
    
    ![](/images/report5.43db3977.png)