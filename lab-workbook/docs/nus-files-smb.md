# Create SMB Share

## Overview

ในแบบฝึกหัดนี้ คุณจะได้สร้างและทดสอบ SMB share ที่ใช้สำหรับรองรับ home directories, user profiles และ unstructured file data อื่นๆ เช่น departmental shares ซึ่งมักจะถูกเข้าถึงโดย Windows clients

## Using SMB Shares

### Creating The Share

1.  เปิด Chrome, ป้อน IP address ของ Prism Central ของคุณ และเข้าสู่ระบบ **Prism Central** โดยใช้ credentials ที่ระบุไว้ให้
    
2.  ไปที่ **Services > Files** และคลิกโดยตรงที่ **BootcampFS**
    
3.  เลือก **Shares & Exports** จากนั้นเลือก **New Share or Export**
    
    ![](/images/1.1a90ed6b.png)
    
4.  กรอกข้อมูลในช่องต่อไปนี้ แล้วคลิก **Next**
    
    -   **Name** - `User##`\-Marketing (เช่น User01-Marketing)
    -   **Description (Optional)** - Departmental share สำหรับ marketing team
    -   **Share Path (Optional)** - ช่องนี้ให้คุณระบุพาธที่มีอยู่เพื่อสร้าง nested share ปล่อยว่างไว้
    -   **Max Size (Optional)** - ช่องนี้ให้คุณตั้งค่า hard quota สำหรับแต่ละ share แยกกัน ปล่อยว่างไว้
    -   **Primary Protocol Access** - SMB
    
    ![](/images/2.dc793f21.png)
    
5.  เลือกตัวเลือกต่อไปนี้ แล้วคลิก **Next**
    
    -   **Enable Self Service Restore**
    -   **Enable Compression**
    
    ![](/images/3.28d1c5bb.png)
    
    เปิดใช้งาน Access Based Enumeration แล้วคลิก **Next**
    
    ![](/images/3a.0d1d5393.png)
    
    **Standard** shares เหมาะสมที่สุดสำหรับตัวอย่างของเรา เนื่องจากคุณกำลังสร้าง departmental share เดี่ยว หมายความว่า top-level directories และไฟล์ทั้งหมดภายใน share รวมถึงการเชื่อมต่อไปยัง share จะถูกจัดการโดย File Server VM (FSVM) เดียว
    
    **Distributed** shares เหมาะสมสำหรับ home directories, user profiles และ application folders ประเภท share นี้จะกระจาย top-level directories ไปยังทุก File Server VMs (FSVM) และทำการ load balance การเชื่อมต่อข้าม FSVMs ทั้งหมดภายใน Files cluster
    
    **Access Based Enumeration (ABE)** ทำให้มั่นใจว่าผู้ใช้จะมองเห็นเฉพาะไฟล์และโฟลเดอร์ที่ผู้ใช้นั้นมีสิทธิ์ (read access) ในการเข้าถึง มักถูกเปิดใช้งานสำหรับ Windows file shares
    
    **Self Service Restore** อนุญาตให้ผู้ใช้ใช้ประโยชน์จาก Windows Previous Version เพื่อกู้คืน (restore) ไฟล์แต่ละไฟล์กลับไปยังเวอร์ชันก่อนหน้าได้อย่างรวดเร็ว โดยอิงจาก Nutanix snapshots
    
6.  ตรวจสอบข้อมูลในหน้า **Summary** แล้วคลิก **Create**
    
    ![](/images/4.4ea31b24.png)
    
7.  เปิดแท็บนี้ทิ้งไว้ เนื่องจากเราจะกลับมาใช้ในภายหลัง
    
### Testing The Share

1.  กลับไปที่ Remote Desktop session ของ `User##`\-WinTools VM ของคุณและเข้าสู่ระบบด้วย credentials ต่อไปนี้:
    
    -   Username **administrator@ntnxlab.local**
    -   Password: **cluster password**

2.  เปิด Windows File Explorer คลิกขวาที่ **SampleData\_Small.zip** ภายในโฟลเดอร์ _Downloads_ และเลือก **Extract All**
    
    ![](/images/6.3c67cd16.png)
    
    -   ผู้ใช้ `NTNXLAB\Administrator` ถูกระบุให้เป็น Files Administrator ระหว่างการติดตั้ง (deployment) Files cluster ทำให้มีสิทธิ์ read/write การเข้าถึง shares ทั้งหมดโดยค่าเริ่มต้น
    -   การจัดการสิทธิ์การเข้าถึงสำหรับผู้ใช้อื่นนั้นไม่แตกต่างจาก SMB share อื่นๆ ทั่วไป

3.  ป้อน `\\BootcampFS.ntnxlab.local\User##-Marketing` (เช่น \\BootcampFS.ntnxlab.local\User01-Marketing) ในช่อง _Files will be extracted to this folder_ แล้วคลิก **Extract** หน้าต่าง Windows File Explorer ใหม่จะเปิดขึ้น
    
    ![](/images/7.58b105b7.png)
    
4.  คลิกขวาที่ **`User##`Marketing > Properties**
    
5.  เลือกแท็บ **Security** และคลิก **Advanced**
    
    ![](/images/8.87acbf50.png)
    
6.  เลือก **Users (BootcampFS\Users)** และคลิก **Remove**
    
    ![](/images/8a.007e813b.png)
    
7.  คลิก **Add**
    
8.  คลิก **Select a principal** และระบุ **Everyone** ในช่อง _Enter the object name to select_ คลิก **OK**
    
    ![](/images/9.09dff69d.png)
    
9.  กรอกข้อมูลในช่องต่อไปนี้และคลิก **OK**:
    
    -   **Type** - Allow
    -   **Applies to** - This folder only
    -   Select **Read & execute**
    -   Select **List folder contents**
    -   Select **Read**
    -   Select **Write**
    
    ![](/images/10.dfcc1bc3.png)
    
10.  คลิก **OK > OK > OK** เพื่อบันทึกการเปลี่ยนแปลง permission
    
    ตอนนี้ผู้ใช้ทุกคนสามารถสร้างโฟลเดอร์และไฟล์ภายใน Marketing share ได้แล้ว
    
    โดยปกติ shares ที่มีผู้ใช้งานหลายคน มักจะใช้ quotas เพื่อให้มั่นใจถึงการใช้ resource อย่างเป็นธรรม Files มีความสามารถในการตั้งค่าทั้ง soft หรือ hard quotas ต่อ share ให้กับผู้ใช้แต่ละรายใน Active Directory หรือ Active Directory Security Groups เฉพาะ
    

### Adding Share Level Quota

1.  กลับไปที่แท็บเบราว์เซอร์ **Nutanix Files**
    
2.  คลิกที่ `User##`**\-Marketing** share เพื่อเปิดดูรายละเอียด share
    
    ![](/images/11.10d6479a.png)
    
3.  ในเมนู _Actions_ เลือก **Add Quota Policy**
    
    ![](/images/13.de2438e0.png)
    
4.  กรอกข้อมูลในช่องต่อไปนี้และคลิก **Add**
    
    -   Select **User Group**
    -   **User or Group** - SSP Developers
    -   **Quota** - 10 GiB
    -   **Enforcement Type** - Hard Limit
    
    ![](/images/14.054f5570.png)
    
    ขั้นตอนเหล่านี้จะบังคับใช้ (enforce) ขีดจำกัด quota บน shares สำหรับ Microsoft Active Directory (AD) user group ที่ชื่อ **SSP Developers** หากใครในกลุ่มนี้ใช้ storage เกิน 10 GB จะไม่สามารถเพิ่มข้อมูลลงใน storage ได้อีก การกำหนด **Soft** limit จะแสดงคำเตือนแต่ไม่มีการจำกัดพฤติกรรมของผู้ใช้แต่อย่างใด
    
5.  ตรวจสอบหัวข้อ **Capacity Summary**, **Performance Summary**, **Share Properties** และ **Features** เพื่อทำความเข้าใจถึงรายละเอียดที่มีอยู่ในแต่ละ share ซึ่งรวมถึง จำนวนไฟล์และการเชื่อมต่อ, storage utilization ในช่วงเวลาต่างๆ, latency, throughput และ IOPS
    
    ![](/images/15.6a15bba2.png)
    
6.  คลิก < ที่มุมซ้ายบนเมื่อคุณพร้อมที่จะดำเนินการในส่วนต่อไป
