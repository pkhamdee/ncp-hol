# Self Service Lab Setup

## Configuring a Project

ผู้ใช้หรือกลุ่มต้องได้รับการกำหนดให้อยู่ใน **Project** ก่อน ซึ่งทำหน้าที่เป็น logical container เพื่อกำหนด role ของผู้ใช้, infrastructure resources, และ resource quotas โดย Project จะเป็นตัวแทนของผู้ใช้ที่มี requirements แบบมาตรฐาน หรือมีโครงสร้างและฟังก์ชันการทำงานทั่วไป เช่น กลุ่มวิศวกรที่ทำงานร่วมกันในโปรเจกต์

ในส่วนนี้ คุณจะได้ทำสิ่งต่อไปนี้:

- สร้าง project
- เพิ่ม Nutanix infrastructure ลงใน project
- กำหนดค่า environment ใน project
- กำหนดสิทธิ์ให้ user group สามารถเข้าถึง project ได้
- สร้าง snapshot policy

### Create a Project

1. เข้าสู่ระบบ Prism Central โดยใช้ `adminuser##` และรหัสผ่าน PC จากหน้า Connection Details

TIP

ตรวจสอบให้แน่ใจว่ามีข้อความ **Login with your Company ID** แสดงอยู่บนหน้าจอเข้าสู่ระบบ

1. เลือก drop-down **App Switcher** ที่ด้านบน แล้วเลือก **Admin Center**
    
    ![](/images/Picture1.99d4d98d.png)
    
2. คลิก **Projects** จากเมนูด้านซ้าย จากนั้นคลิกปุ่ม **Create Project**
    
    ![](/images/Picture2.ab0bbbcb.png)
    
3. ในช่อง **Project Name** ให้กรอก `User##`-Project จากนั้นคลิก Create
    
    ![](/images/Picture3.2c164cba.png)
    

### Add Infrastructure

1. ในส่วนของ **Summary** ให้คลิกที่ **+ Add Infrastructure** และกรอกข้อมูลในช่องด้านล่าง
    
    ![](/images/Picture4.f8398e6a.png)
    
    - คลิกที่ปุ่ม drop-down **Add Infrastructure** แล้วเลือก `NTNX_LOCAL_AZ`
    - คลิก _Configure Resources_
    - เลือก cluster ของคุณใน drop-down **Select clusters** เพื่อเพิ่มลงใน project นี้
    - คลิก **Select VLANs**
    - ทำเครื่องหมายถูกที่กล่อง primary
    - คลิก **Confirm** และ **Select Default**
    - คลิกที่รูป `star` ในคอลัมน์ **Default**
    - คลิก **Confirm** > **Save**
    - คลิกปุ่ม **Save** จากมุมขวาบน
    
    ![](/images/Picture4a-InfrastructureSummary.1767d57a.png)
    

### Configure an Environment

Environment คืออะไร?

คุณสามารถกำหนด environments ได้หลายรูปแบบ (เช่น Dev หรือ Prod) ในโปรเจกต์เดียวกัน Environment จะช่วยให้คุณสามารถกำหนด infrastructure และ configurations เฉพาะเจาะจงได้ขึ้นอยู่กับ environment ที่ใช้ในการ deployment

1. จากเมนูด้านบน ให้เลือก **Environments** แล้วกรอกข้อมูลต่อไปนี้:
    
    - คลิก **Create Environment**
    - กรอก `User##`-Environment ในช่อง _Name_ แล้วคลิก **Next**
    - คลิกที่ drop-down **Select Infrastructure** แล้วเลือก **NTNX_LOCAL_AZ**
    - คลิกที่ใดก็ได้ในกล่อง _VM Configuration_ เพื่อขยายหน้าต่าง
        - ดำเนินการตามขั้นตอนต่อไปนี้ในส่วนของ _Windows_
            - เลือก cluster ของคุณจาก drop-down _Cluster_
            - ในส่วนของ _VM Configuration_ ให้ระบุข้อมูลต่อไปนี้:
                - **vCPUs** - 2
                - **Cores per vCPU** - 1
                - **Memory (GiB)** - 4
            - เลื่อนลงมาที่ส่วน _Disks_ และเลือก **WindowsServer2022.qcow2** จาก drop-down _Image_
            - เลื่อนลงมาที่ส่วน _Network Adapters (NICs)_ และคลิก (ปุ่มเพิ่ม)
                - เลือก **Primary** จาก drop-down _NIC 1_
        - เลื่อนกลับไปด้านบนของหน้าจอ และดำเนินการตามขั้นตอนต่อไปนี้ในส่วนของ _Linux_
            - เลือก cluster ของคุณจาก drop-down _Cluster_
            - ในส่วนของ _VM Configuration_ ให้ระบุข้อมูลต่อไปนี้:
                - **vCPUs** - 2
                - **Cores per vCPU** - 1
                - **Memory (GiB)** - 4
            - เลื่อนลงมาที่ส่วน _Disks_ และเลือก **Rocky9.qcow2** จาก drop-down _Image_
            - เลื่อนลงมาที่ส่วน _Network Adapters (NICs)_ และคลิก
                - เลือก **Primary** จาก drop-down _NIC 1_
        - คลิก **Next > Add Credential** แล้วกรอกข้อมูลในช่องต่อไปนี้:
            - **Name** - WINDOWS
            - **Username** - `administrator`
            - **Secret Type** - Password
            - **Password** - `nutanix/4u`
        - คลิก **Add Credential**
            - **Name** - ROCKY
            - **Username** - `rocky`
            - **Secret Type** - Password
            - **Password** - `nutanix/4u`
        - คลิก **Save Environment & Project**

หน้าจอของคุณควรจะดูคล้ายกับรูปด้านล่างนี้:

![](/images/Picture4b-EnvironmentSummary.3bae7a66.png)

### Assign a User Group

1. จากเมนูด้านบน เลือก **Users & Groups**
    - คลิก **Add/Edit Users & Groups**
        
    - คลิก **Add User**
        
        - **Name** - Administrators (group)
        - **Role** - Project Admin
        
        ![](/images/Picture5.3db23804.png)
        
    - คลิก **Save Users and Project**
        

### Create a Snapshot Policy

Snapshot Policy คืออะไร?

Snapshot policy ช่วยให้คุณสามารถกำหนด rules ในการสร้างและจัดการ snapshots ของ VMs ได้

1. ในเมนูด้านบน ให้คลิกที่ **Policies** และเลือก **Snapshot** จากเมนูด้านซ้าย
    
2. คลิก **Create Snapshot Policy**
    
3. กรอก `User##`-**Snapshot-Policy** ในส่วนของ Policy Name และเลือก `User##`-**Environment** ใน dropdown ของ **Primary Site**
    
4. คลิก **Save Snapshot Policy**
    

คุณได้เสร็จสิ้นกระบวนการ setup แล้ว และสามารถไปยังส่วนใดส่วนหนึ่งหรือทั้งหมดของเนื้อหาเหล่านี้ได้เลย:

- [Calm IaaS: Linux](/calm_iaas_linux.html)
- [Calm IaaS: Windows](/calm_iaas_windows.html)
- [Calm DSL](/calm_dsl.html)
