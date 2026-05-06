# Setting Up an Approval Policy

Nutanix Self-Service ช่วยให้คุณสามารถสร้าง approval policies สำหรับ runbook executions, app launch และ app day-2 operations (system-defined หรือ user-defined actions) แต่ละ approval policy คือชุดของ conditions ที่ถูกกำหนดไว้ซึ่งคุณนำไปปรับใช้กับ entities เฉพาะใน Self-Service ตัว approval request จะถูกสร้างขึ้นเมื่อ event ที่เกี่ยวข้องตรงตาม conditions ทั้งหมดที่กำหนดไว้ใน policy

## Creating an Approval Policy

1.  Login เข้าสู่ Prism Central โดยใช้ **adminuser`##`** และ PC password จากหน้า Connection Details
    
2.  ไปที่ส่วน App Switcher ที่มุมซ้ายบนของ Prism Central คลิก **Self Service** ใน App Switcher
    
3.  คลิก **Policies** ใน toolbar ด้านซ้าย
    
4.  คลิก **Create Approval Policy**
    
5.  มาสร้าง approval policy สำหรับ **Linux IaaS Blueprint Application** กัน
    
6.  กรอกข้อมูลด้านล่าง:
    
    -   Name - **User`##`_Linux_IaaS_Policy** โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย
    -   **Project - `Initials`-FiestaProject**
    -   ในส่วน **When does this policy get triggered?** ให้กรอกฟิลด์ด้านล่าง
        -   **Entity Type**: เลือก Application จาก drop-down
        -   **Action: Select** Launch จาก drop-down

!!! note
    Project ควรเป็นอันเดียวกับที่คุณสร้างไว้ก่อนหน้านี้ในส่วนของแบบฝึกหัด Projects

![](/images/selfservice12.070ce669.png)

7.  คลิก **Next**

## Set Conditions And Approvers

1.  กรอกฟิลด์ต่อไปนี้ในหน้า **Set Conditions**
    
    -   Condition 1
    -   Attribute: เลือก Marketplace Item Name จาก drop-down
    -   Operator: เลือก Contains จาก drop-down
    -   Value: พิมพ์ CentOS
2.  คลิก **Next**
    
3.  กรอกฟิลด์ต่อไปนี้ในหน้า **Select Approvers**
    
    -   Set 1
    -   Set Name: Set 1
    -   Approval Rule: Any one can approve
    -   Approvers: เลือก admin(person) จาก drop-down
4.  คลิก **Save**
    
5.  คลิก **Yes, enable this policy**.
    
6.  มาสร้าง approval policy อีกอันสำหรับ **Windows IaaS Blueprint Application**
    
7.  คลิก **Create Approval Policy**
    
8.  กรอกข้อมูลด้านล่าง:
    
    -   Name - **User`##`_Windows_IaaS_Policy** โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย
    -   **Project - `Initials`-FiestaProject**
    -   ในส่วน When does this policy get triggered? ให้กรอกฟิลด์ด้านล่าง
        -   **Entity Type**: เลือก Application จาก drop-down
        -   **Action**: เลือก Launch จาก drop-down

!!! note
    Project ควรเป็นอันเดียวกับที่คุณสร้างไว้ก่อนหน้านี้ในส่วนของแบบฝึกหัด Projects

9.  คลิก **Next**
    
10.  กรอกฟิลด์ต่อไปนี้ในหน้า **Set Conditions**
    
    -   Condition 1
    -   Attribute: เลือก Marketplace Item Name จาก drop-down
    -   Operator: เลือก Contains จาก drop-down
    -   Value: พิมพ์ Windows
11.  คลิก **Next**
    
12.  กรอกฟิลด์ต่อไปนี้ในหน้า **Select Approvers**
    
    -   Set 1
    -   Set Name: Set 1
    -   Approval Rule: Any one can approve
    -   Approvers: เลือก admin(person) จาก drop-down
13.  คลิก **Save**
    
14.  คลิก **Yes, enable this policy**.
    

## Takeaways

คุณจะเห็น approval policy ทำงานเมื่อ developer หรือ consumer ทำการ launch ตัว application จาก Self-Service Marketplace
