# Identity and Access Management (IAM)

Identity and Access Management (IAM) คือฟีเจอร์สำหรับการ authentication และ authorization ที่ใช้ประโยชน์จากการควบคุมการเข้าถึงแบบ Role-Based Access Control (RBAC) ที่มีความละเอียดอ่อน (fine-grained) ร่วมกับ Authorization Policies. IAM ช่วยให้ผู้ใช้สามารถใช้งาน system-defined roles หรือสร้าง custom roles ตาม operations และ permissions แบบเฉพาะเจาะจงได้

ในส่วนนี้ คุณจะได้เรียนรู้วิธีการใช้ประโยชน์จาก system roles, ทำการ assign ให้กับ user, และสังเกตการทำงานของมัน

# Identity and Access Management Fundamentals

![](/images/IAM28.64aedc30.png)

# Overview of the IAM Interface

IAM ถูกจัดการผ่าน Admin Center ของ Prism Central

1.  Login เข้าสู่ Prism Central โดยใช้ `adminuser##` และ PC password จากหน้า Connection Details
    
2.  ไปที่ส่วนของ App Switcher ที่มุมบนซ้ายของ Prism Central คลิก **Admin Center** ใน App Switcher
    
    ![](/images/IAM1.ab9305ef.png)

3.  คลิก **IAM** ใน Admin Center Dashboard

    ![](/images/IAM2.ab00e124.png)

4.  ในส่วน **Authorization Policies** เราสามารถเห็น system-defined policies บางส่วน System-defined policies เคยถูกเรียกว่า Role Mappings ใน Prism เวอร์ชันก่อนหน้า
    
    Prism Admin และ Prism Viewer policies สามารถถูก duplicated ได้ (แต่ไม่สามารถ edited หรือ deleted ได้), Super Admin policy ไม่สามารถถูก duplicated ได้ อย่างไรก็ตาม คุณสามารถสร้าง authorization policy ใหม่ที่รวมเอา Super Admin role เข้าไปได้
    
    ![](/images/IAM3.d4869c89.png)

5.  ส่วน **Roles** จะแสดงภาพรวมของทั้ง built-in และ custom roles ซึ่งรวมถึงรายละเอียดต่างๆ เช่น role name, description, services และ entities ที่สามารถเข้าถึงได้, และวันที่อัปเดตล่าสุด
    
    System-defined roles สามารถถูก duplicated หรือเพิ่มเข้าไปใน authorization policy ได้ แต่ไม่สามารถ modified หรือ deleted ได้ ปุ่ม **Create Role** ช่วยให้คุณสร้าง role ใหม่ตั้งแต่ต้น (from scratch) ได้
    
    ![](/images/IAM4.6931a15a.png)

6.  แท็บ **Identities** ให้มุมมองของ local users, imported users, และ user groups โดย Imported users คือ users ที่ถูก imported มาจาก Active Directory, Open LDAP, และ SAML Identity Providers

    ![](/images/IAM5.15fbe755.png)

7.  แท็บ **IdP Configuration** จะแสดงตัวเลือก identity provider (IdP) ที่มีให้ใช้งาน คุณสามารถเพิ่ม Active Directory, OpenLDAP-based provider, หรือ SAML identity provider ได้
    
    นอกจากนี้ คุณยังสามารถ configure ตัว Common Access Card (CAC) authentication ได้ ตัวเลือก Download Metadata ช่วยให้คุณได้รับไฟล์ XML ที่อธิบายเกี่ยวกับ Prism Central ซึ่งสามารถนำไป uploaded ลงใน identity provider ได้
    
    ![](/images/IAM6.d61a65c9.png)

## Delegating with Authorization Policies

ลองพิจารณา IT organization ที่มี IT Operator Role ตัว IT Operator จะมี permissions ที่จำกัด โดยอนุญาตให้พวกเขาสามารถ view VMs, power on, และ revert พวกมันได้ actions ทั้งหมดนี้ถูกจำกัดไว้สำหรับ VMs ที่อยู่ใน category ที่ระบุชื่อว่า User01:Production เท่านั้น ตอนนี้มาลองดูวิธีการใช้ system-defined roles เพื่อจัดการ tasks เหล่านี้กัน

Prism มี roles มากมายที่ถูกกำหนดไว้ล่วงหน้า (predefined) เช่น VM Operator, Network Admin, Prism Admin, Prism Viewer

1.  Login เข้าสู่ Prism Central โดยใช้ `adminuser##` และ PC password จากหน้า Connection Details
    
2.  ไปที่ส่วนของ **App Switcher** ที่มุมบนซ้ายของ Prism Central คลิก **Admin Center** ใน App Switcher
    
3.  คลิก **IAM** ใน Admin Center Dashboard
    
4.  คลิกแท็บ **Roles**
    
5.  ค้นหา Role Name: **Virtual Machine Operator**
    
    ![](/images/IAM7.be5a1011.png)

6.  เลือก Role และคลิก **Actions**
    
7.  เลือก **Add Authorization Policy**

    ![](/images/IAM8.007751f7.png)

8.  คลิกที่ **Pencil icon** (ไอคอนดินสอ) ถัดจาก **Virtual Machine Operator Policy** และเปลี่ยนชื่อเป็น "Virtual Machine Operator User`##`" โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย นี่คือ shared cluster ดังนั้นอย่าลืมเปลี่ยนชื่อ policy

    ![](/images/IAM29.267212ae.png)

9.  คุณสามารถดูรายการของ operations ที่ได้รับอนุญาตสำหรับ role นี้ได้

    ![](/images/IAM9.ac63fdb0.png)

10.  คลิก **Next** เมื่อคุณพร้อมที่จะดำเนินการกำหนด scope
    
11.  คุณสามารถเลือกระหว่าง Full Access (ทุก entity types และ instances) หรือ Configure Access (เลือก entity types และ instances ที่เฉพาะเจาะจง) เนื่องจากเรากำลังทำการ filtering ด้วย User`##`:Production โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย ให้ทำตามขั้นตอนต่อไปนี้ให้เสร็จสมบูรณ์:
    
    1.  เลือก **Configure Access : Select entity types and instances**
    2.  เลือก **All Entity types**
    3.  เลือก **In Category** ในฟิลด์ Filters และ **ค้นหา "User`##`:Production" โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย**
    4.  ในตัวอย่างนี้เราจะค้นหา User01:Production Category

    ![](/images/IAM10.ba0dd8b1.png)

12.  คลิก **Next** เมื่อคุณพร้อมที่จะดำเนินการ Assign Users
    
13.  ให้เลือก ntnxlab(AD) และเลือก AD user เป็น operator`##`@ntnxlab.local
    
    ![](/images/IAM11.0c2fcc99.png)

14.  คลิก Save คุณควรเห็นข้อความ "Policy created Successfully" เมื่อการทำงานสำเร็จ

## Authorization Policy Verification

1.  ตอนนี้ ให้ login เข้าสู่ Prism Central ด้วย AD user operator`##`@ntnxlab.local โดยใช้ PC password จากหน้า Connection Details
    
2.  ใน Infrastructure Dashboard ขยาย Compute & Storage คลิก VMs
    
    ![](/images/IAM12.f0d474b5.png)

3.  VMs ที่อยู่ใน Category User`##`:Production จะถูกแสดงอยู่ที่นี่

    ![](/images/IAM14.3a0b7360.png)

4.  ให้ทำการเลือก VM และคลิก **Actions**
    
5.  เราสามารถเห็นได้ว่า user สามารถทำ limited operations ได้เท่านั้น เช่น Update, Power On, และ Create Recovery Point
    
    ![](/images/IAM15.2e10161b.png)