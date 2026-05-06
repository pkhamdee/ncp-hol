# Nutanix Cloud Platform

## Overview of Categories

Categories เป็นรากฐานของการจัดการ cloud operating model สิ่งเหล่านี้คือระบบ labeling ที่ช่วยให้คุณสามารถ tag resources (เช่น VMs, images, subnets และ clusters) ด้วย metadata โดย category จะช่วยจัดกลุ่มและจัดระเบียบ infrastructure เพื่อให้การทำ filtering, policy enforcement และ automation ง่ายขึ้น

Category จะจัดกลุ่ม entities เป็น key-value pairs ทำให้สามารถทำ assignment ตามเกณฑ์ที่ระบุได้ Policies สามารถถูกนำไปใช้กับ entities ที่ถูกจัดกลุ่มตาม category value

ตัวอย่างเช่น category key "Department" ที่มี values เช่น Engineering, Finance และ HR สามารถรองรับ backup policies ที่แตกต่างกันได้: แบบหนึ่งสำหรับ engineering และ HR และอีกแบบที่เข้มงวดกว่าสำหรับ finance

Prism Central ช่วยให้มองเห็นภาพรวม (visualization) ของ relationships เหล่านี้ได้อย่างรวดเร็ว

ในส่วนนี้ คุณจะได้เรียนรู้วิธีสร้าง category และนำไปใช้กับ entities (ซึ่งในตัวอย่างนี้คือ VMs)

## Create a Category

1.  เข้าสู่ระบบ Prism Central โดยใช้ `adminuser##` และ PC password ที่ให้ไว้ แทนที่ `##` ด้วยหมายเลขผู้ใช้ (user number) ของคุณจากรายละเอียดที่ให้ไว้
    
    !!! tip
        ตรวจสอบให้แน่ใจว่าข้อความ **Login with your Company ID** แสดงอยู่บนหน้าจอ login
    
2.  โปรดเลือก drop-down App Switcher ที่ด้านบนแล้วเลือก Admin Center
    

![App Switcher Selection](/images/appswitcher1.4b238747.png)

3.  ดำเนินการเลือก Categories จากเมนูด้านซ้าย
    
4.  Prism Central มี system categories ที่กำหนดไว้ล่วงหน้า (predefined) หลายรายการ ซึ่งไม่สามารถทำ update หรือ delete ได้ คุณสามารถ assign entities ให้กับ system categories เหล่านี้ได้ในขณะที่สร้างหรืออัปเดต VMs และ entities อื่นๆ
    

![Category List](/images/category1.0f8ae463.png)

5.  คลิก **New Category**
    
6.  กำหนด parameters ของ category
    
    -   Name: **User`##`** โดยที่ `##` คือ `User #` ที่คุณได้รับมอบหมายจาก Connection Details
        
    -   Purpose: ให้ข้อมูล purpose (ทางเลือก) สำหรับ category ของคุณ
        
    -   Values: **Production**
        
    -   คลิก **Save**
        
    
    ![Category Key and Values](/images/category2.5c3357a0.png)
    

## Assign Category to VMs

เมื่อกำหนด category ของเราแล้ว มาลองนำไปใช้กับ virtual machines กัน การทำเช่นนี้จะเป็นการตั้งค่า category ให้เป็น metadata สำหรับ VMs ที่ถูก assigned

1.  จาก drop-down App Switcher menu ด้านบน ให้คลิกที่ **Infrastructure** จากนั้นคลิกที่ **Compute > VMs** จากเมนูด้านซ้าย
    
2.  ใช้ filter bar เพื่อค้นหา VMs ที่ตรงกับ `User #` ของคุณ
    
    -   พิมพ์ **##** โดยที่ **##** ตรงกับ user number ของคุณ
    -   จากนั้นคลิกที่ **Name Contains '##'**
    
    ![Find VMs](/images/categorytovm1.c5a97f63.png)
    
3.  คุณจะเห็น 2 VMs ที่ตรงกับ user number ของคุณ:
    
    -   Desktop##
    -   User##-Move
    
    ![Found VMs](/images/categorytovm2.b4d86ec1.png)
    
4.  เลือกทั้งสอง VMs
    
5.  คลิกที่ drop-down **Actions** จากนั้นไปที่ **Other Actions > Manage Categories**
    
    ![Manage Categories](/images/categorytovm3.a4abf2ba.png)
    
6.  ในช่อง Search ให้ค้นหา category ใหม่ของคุณคือ **User`##`: Production**
    

!!! note
    คุณสามารถประหยัดเวลาได้โดยการพิมพ์ตัวอักษรเฉพาะไม่กี่ตัวใน query ของคุณเพื่อใช้ประโยชน์จากการทำ auto-populate

![Find Category](/images/categorytovm4.9652c95a.png)

1.  คลิก **Save** เพื่อใช้ category นี้กับสอง VMs ของคุณ

ตอนนี้ VMs ของคุณถูก assigned ให้กับ category ใหม่ของคุณแล้ว คุณจะเห็นว่ายังไม่มี policies ใดที่เกี่ยวข้องกับ VMs เหล่านี้ตาม categories เหล่านี้ เราจะพูดถึงเรื่องนี้ในขั้นตอนถัดไป
