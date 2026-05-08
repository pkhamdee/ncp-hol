# Prism Central Overview

ในส่วนนี้ คุณจะได้เรียนรู้เกี่ยวกับ interface ของ Prism Central (PC) ทำความคุ้นเคยกับ layout และ navigation ของมัน Prism Central เป็น configuration portal หลักสำหรับ Flow Network Security policies และ categories

## Prism Central

Prism Central เป็น centralized management platform ที่ช่วยให้คุณสามารถจัดการ multiple Nutanix clusters ได้จาก single pane of glass มันมี central dashboard สำหรับ clusters, VMs (Virtual Machines), hosts, disks และ storage พร้อมความสามารถในการ drill-down เพื่อดูข้อมูลโดยละเอียด คุณสามารถทำการ centrally configure แต่ละ individual clusters และใช้ single sign-on สำหรับ Prism Element clusters ทั้งหมดที่ลงทะเบียนไว้ได้

จาก Chrome web browser ภายใน virtual desktop ของคุณ ให้เชื่อมต่อกับ Prism Central โดยใช้ IP ที่อยู่ใน connection details ที่ instructor ของคุณให้ไว้

1.  ใส่ `PC-IP-ADDRESS` ใน browser tab ใหม่
    
2.  ยอมรับ certificate warnings ใดๆ โดยคลิกที่ **Advanced** และ **Proceed**
    
3.  Log in เข้าสู่ Prism Central ตรวจสอบให้แน่ใจว่าหน้าจอมีคำว่า **Login with your Company ID** อยู่ด้านบน
    
    -   **username** - `<PC username> ในรูปแบบ adminuser##@ntnxlab.local`
    -   **password** - `<PC password ที่ให้ไว้>`
    
    ![](/images/pc-login.fc4dd230.png)
    
    !!! note
        หากนี่เป็นครั้งแรกที่ logging into เข้าสู่ Prism Central คุณอาจเห็น guided tour คุณสามารถปิดได้โดยเลือก **Skip the Tour**

4.  หลังจากที่คุณ log in เข้าสู่ PC แล้ว ให้ทำความคุ้นเคยกับ interface นี่คือ main dashboard ของ PC ซึ่งจะให้ overview ของ clusters ทั้งหมดที่ลงทะเบียนกับ PC instance นี้
    
    ![](/images/pc-login-dashboard.f6ec78f9.png)
    
5.  ตรวจสอบหน้าจอ widgets และระบุ items ต่อไปนี้:
    
    -   Alerts
    -   Tasks

6.  ถัดไป ให้ดูที่ส่วน App Switcher ที่ด้านบนซ้ายของ Prism Central
    
    !!! note
        คุณสามารถใช้ App Switcher เพื่อ navigate อย่างรวดเร็วและเข้าถึง capabilities ของ Nutanix Cloud Platform ที่ enabled ผ่าน Prism Central ตัว cluster ของคุณอาจไม่มี services ที่ enabled มากเท่าที่แสดงในนี้
    
    ![](/images/pc-login-app-switcher2.68aa5e91.png)
    
7.  ขยาย hamburger menu เพื่อดู options ต่างๆ สำหรับส่วนนั้น ตัวอย่างเช่น สำหรับ Infrastructure คุณสามารถเข้าถึงทุกสิ่งที่เกี่ยวกับ infrastructure เช่น:
    
    -   Compute
    -   Storage
    -   Network & Security
    -   Data Protection
    
    ![](/images/pc-login-app-switcher1.917cbb33.png)
    

เราจะเจาะลึกบางส่วนเหล่านี้ใน bootcamp นี้ แต่คุณสามารถ browse และคลิกดูบางส่วนได้ตามสบาย

เราจะโฟกัสไปที่ **Network & Security** ซึ่งเป็นที่ที่ Flow Network Security policies ถูก configured ไว้ใน Infrastructure ส่วน Categories จะถูก managed ภายใน **Admin Center** ซึ่งเป็นอีกส่วนหนึ่งของ Prism Central ที่สามารถเข้าถึงได้ผ่าน App Switcher

## Takeaways

-   Prism Central คือ main control pane ที่คุณใช้โต้ตอบ (interact) และใช้งาน Nutanix Cloud Platform ของคุณ รวมถึง Flow Network Security policies ด้วย
