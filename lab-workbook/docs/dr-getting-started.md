# Getting Started with Disaster Recovery

## Overview

ใน lab นี้ คุณจะได้:

-   ใช้งาน multi-tier application - Fiesta - ที่ถูก deploy โดย Calm
-   ปกป้อง VMs ของคุณด้วย Protection Policy
-   สร้าง Recovery Plan สำหรับ runbook automation
-   สร้างการเปลี่ยนแปลงให้กับ Fiesta application database
-   ดำเนินการ failover ไปยัง physical Nutanix cluster
-   เข้าถึง Fiesta application เพื่อให้แน่ใจว่าการเปลี่ยนแปลงถูกเก็บรักษาไว้
-   ทำการเปลี่ยนแปลงเพิ่มเติมกับ Fiesta application database
-   ดำเนินการ failback จาก physical Nutanix cluster
-   เข้าถึง Fiesta application เพื่อให้แน่ใจว่าการเปลี่ยนแปลงถูกเก็บรักษาไว้

Bootcamp นี้มีแบบฝึกหัดสำหรับทั้ง failover scenarios แบบ _Unplanned_ และ _Planned_ คุณจะใช้ VMs, Protection Policy, และ Recovery Plan ตัวเดียวกันสำหรับทั้งสองแบบฝึกหัด ซึ่งมีรายละเอียดในบทนำของแต่ละ lab เช่นกัน เมื่อคุณทำแบบฝึกหัดหนึ่งเสร็จแล้ว คุณสามารถข้าม configuration steps สำหรับแบบฝึกหัดที่สองได้

## What's New

Bootcamp นี้ได้รับการอัปเดตให้ใช้ software versions ต่อไปนี้:

-   AOS - 6.5.2.5
-   AHV - el7.nutanix.20220304.342
-   Prism Central - pc.2022.6.0.3

## Environment Requirements

-   4-node AHV clusters จำนวนสองชุดใน HPOC data center เดียวกัน เพื่อให้ตรงตาม synchronous replication latency requirements (<5ms RTT)
    
    !!! note    
        ห้ามผสม single-node และ 4-node clusters ใน Bootcamp environment นี้ ตัว Recovery Plans ของ Disaster Recovery ต้องการให้ทั้งสอง clusters มี network prefix length ที่เหมือนกัน (เช่น /25, /26) นอกจากนี้ เนื่องจากสิ่งนี้มีจุดประสงค์เพื่อเป็น add-on Bootcamp จึงจำเป็นต้องใช้ 4-node clusters
    
-   คุณสามารถรัน standard Bootcamp staging ใดก็ได้บนคลัสเตอร์หนึ่ง ซึ่งจะทำหน้าที่เป็น _Recovery_ site cluster ของคุณ
    
-   คุณต้องรัน _Disaster Recovery Add-On Bootcamp_ staging บนคลัสเตอร์เพิ่มเติมของคุณเพื่อใช้เป็น _Primary_ site cluster
    
-   ทำตามขั้นตอนด้านล่างเพื่อเปิด required ports: 2030, 2036, 2073, และ 2090
    
!!! note    
    ขั้นตอนนี้มีความจำเป็นแม้ว่าจะใช้สอง HPOC clusters ใน data center เดียวกันก็ตาม
    
    1.  ในการเปิด ports สำหรับ communication กับ recovery cluster ให้รันคำสั่งต่อไปนี้บน CVM สำหรับ _Primary_ site cluster โดยการรีโมทผ่าน SSH (`ssh nutanix@<CLUSTER-VIRTUAL-IP>`):
        
        ```
        allssh 'modify_firewall -f -r recovery_cvm_ip,recovery_virtual_ip -p 2030,2036,2073,2090 -i eth0'
        ```
        
        แทนที่ `recovery_cvm_ip` ด้วย IP address ของ Recovery cluster CVMs โดยคั่นด้วยเครื่องหมายจุลภาค (comma)
        
        แทนที่ `recovery_virtual_ip` ด้วย virtual IP address ของ Recovery cluster
        
    2.  ในการเปิด ports สำหรับ communication กับ primary cluster ให้รันคำสั่งต่อไปนี้บน CVM สำหรับ _Recovery_ site cluster โดยการรีโมทผ่าน SSH (`ssh nutanix@<CLUSTER-VIRTUAL-IP>`):
        
        ```
        allssh 'modify_firewall -f -r primary_cvm_ip,primary_virtual_ip -p 2030,2036,2073,2090 -i eth0'
        ```
        
        แทนที่ `primary_cvm_ip` ด้วย IP address ของ Primary cluster CVMs โดยคั่นด้วยเครื่องหมายจุลภาค (comma)
        
        แทนที่ `primary_virtual_ip` ด้วย virtual IP address ของ Primary cluster
        
    3.  ออกจากทั้งสอง SSH sessions
        

!!! note    
    สำหรับข้อมูลเพิ่มเติมเกี่ยวกับ required ports โปรดดูหัวข้อ **General Requirements of Disaster Recovery** ภายใน [Disaster Recovery (Formerly Leap) 6.1 Administration Guideopen in new window](https://portal.nutanix.com/page/documents/details?targetId=Leap-Xi-Leap-Admin-Guide-v2022_6:Leap-Xi-Leap-Admin-Guide-v2022_6)
    
-   ตัว staging script นี้จะทำการ deploy ตัว instances ของ Fiesta application จำนวนสิบตัวโดยอัตโนมัติ:
    
    -   **User01-MYSQL-...**; **User01-WebServer-...**
    -   **User02-MYSQL-...**; **User02-WebServer-...**
    
-   ผู้สอนควรกำหนดหมายเลข _User_ ให้กับผู้เข้าร่วมแต่ละคน ตัว lab guide จะอ้างอิงถึง entity names ด้วย _User##_ ซึ่งควรนำหมายเลขเฉพาะของตนเองไปแทนที่ (เช่น _User01_)
    
-   ขอแนะนำให้เปลี่ยนชื่อ clusters ภายใน Prism เป็น _Primary_ และ _Recovery_ ตามลำดับ เพื่อช่วยในการระบุตัวตน (identification) ในระหว่างทำ labs
