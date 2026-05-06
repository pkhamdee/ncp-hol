# Identity and Access Management (IAM)

Identity and Access Management (IAM) เป็นฟีเจอร์สำหรับ authentication และ authorization ซึ่งใช้ประโยชน์จาก Role-Based Access Control (RBAC) แบบละเอียดร่วมกับ Authorization Policies เพื่อกำหนดว่า **ใคร** สามารถทำ **action อะไร** กับ **entities ใด** ได้บ้าง IAM ยังเปิดโอกาสให้ผู้ใช้สามารถใช้ roles ที่ระบบกำหนด (system-defined roles) หรือสร้าง custom roles ตาม operations และ permissions ที่กำหนดรายละเอียดได้อย่างเจาะจง

ในส่วนนี้ คุณจะได้เรียนรู้วิธีการใช้ประโยชน์จาก system roles, การ assign roles เหล่านี้ให้กับ user และสังเกตการทำงานของระบบ

![IAM Fundamentals](/images/IAM28.64aedc30.png)

## Overview of the IAM Interface

IAM จะถูกจัดการผ่าน Admin Center ของ Prism Central

## Navigating to IAM

1.  Login เข้าสู่ Prism Central โดยใช้ `adminuser##` และ PC password จากหน้า Connection Details
    
2.  ไปที่ส่วน App Switcher ที่มุมบนซ้ายของ Prism Central คลิก **Admin Center** ใน App Switcher
    

![App Switcher](/images/IAM1.ab9305ef.png)

3.  คลิก **IAM** ใน Admin Center Dashboard

![IAM Menu](/images/IAM2.ab00e124.png)

## Overview of the Interface

Authorization Policies เป็นส่วนที่เชื่อมโยง Identity and Access Management เข้าด้วยกัน authorization policies จะรวมคำจำกัดความของ role, scope และ users เพื่อให้ได้ชุดของ permissions ที่ได้รับอนุญาต (allowed permissions)

ในส่วน **Authorization Policies** เราสามารถดู policies ที่ระบบกำหนด (system-defined policies) ได้ system-defined policies เคยถูกเรียกว่า Role Mappings ใน Prism เวอร์ชันก่อนหน้า

Prism Admin และ Prism Viewer policies สามารถถูก duplicate ได้ (แต่ไม่สามารถแก้ไขหรือลบได้) ส่วน Super Admin policy ไม่สามารถ duplicate ได้ อย่างไรก็ตาม คุณสามารถสร้าง authorization policy ใหม่ที่รวมเอา Super Admin role เข้าไปได้

![Existing Authorization Policies](/images/IAM3.d4869c89.png)

ส่วน **Roles** แสดงภาพรวมของทั้ง built-in และ custom roles รวมถึงรายละเอียดต่างๆ เช่น ชื่อ role, คำอธิบาย, services และ entities ที่สามารถเข้าถึงได้ และวันที่อัปเดตล่าสุด Roles จะประกอบด้วยประเภทของ allowed actions ซึ่งสามารถกำหนดได้อย่างละเอียดมาก

System-defined roles สามารถถูก duplicate หรือเพิ่มเข้าใน authorization policy ได้ แต่จะไม่สามารถแก้ไขหรือลบได้ ปุ่ม **Create Role** จะช่วยให้คุณสร้าง role ใหม่ได้ตั้งแต่ต้น (from scratch)

![Existing Roles](/images/IAM4.6931a15a.png)

แท็บ **Identities** จะแสดงมุมมองของ local users, imported users และ user groups โดย imported users คือ users ที่ถูกนำเข้าจาก Active Directory, Open LDAP และ SAML Identity Providers

![Identities](/images/IAM5.15fbe755.png)

แท็บ **IdP Configuration** แสดงตัวเลือก identity provider (IdP) ที่ใช้งานได้ คุณสามารถเพิ่ม Active Directory, OpenLDAP-based provider หรือ SAML identity provider ได้

นอกจากนี้ คุณยังสามารถ configure การตรวจสอบสิทธิ์แบบ Common Access Card (CAC) ได้ ตัวเลือก Download Metadata จะช่วยให้คุณสามารถดาวน์โหลดไฟล์ XML ที่อธิบายเกี่ยวกับ Prism Central ซึ่งจากนั้นสามารถอัปโหลดไปยัง identity provider ได้

![IdP Configuration](/images/IAM6.d61a65c9.png)

## Delegating with Authorization Policies

องค์กรไอที (IT organization) ของเราต้องการสร้าง IT Operator Role แบบจำกัด (limited IT Operator Role) IT Operator ควรมี limited permissions ซึ่งอนุญาตให้พวกเขาดู VMs, เปิดใช้งาน (power them on) และ revert ได้ การ actions ทั้งหมดนี้จะถูกจำกัดเฉพาะกับ VMs ที่อยู่ใน category ที่กำหนดชื่อว่า **User`##`:Production** ตอนนี้เรามาสำรวจวิธีใช้ system-defined roles เพื่อ manage tasks เหล่านี้กัน

Prism มีชุด roles ที่กำหนดไว้ล่วงหน้า (predefined) จำนวนมาก เช่น: VM Operator, Network Admin, Prism Admin, Prism Viewer

1.  จากหน้าต่าง IAM interface ให้คลิกที่แท็บ **Roles**
    
    !!! tip
        เคล็ดลับ หากต้องการไปที่หน้าต่าง IAM interface ดูที่ส่วน IAM
    
2.  ค้นหา Role Name: **Virtual Machine Operator**
    
    ![Role Search](/images/IAM7.be5a1011.png)
    
3.  เลือก Role และคลิก **Actions**
    
4.  เลือก **Add Authorization Policy**
    
    ![Add Auth Policy](/images/IAM8.007751f7.png)
    
    !!! tip
        หากมีหน้าต่างป๊อปอัป "Getting started" ปรากฏขึ้น ให้คลิก **Skip for now** ![Skip for now](/images/IAMpopup.7bc91ddb.png)
    
5.  คลิก **ไอคอนดินสอ (Pencil icon)** ถัดจาก **Virtual Machine Operator Policy** และเปลี่ยนชื่อเป็น "Virtual Machine Operator User`##`" โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย เนื่องจากนี่คือ shared cluster ดังนั้น **อย่าลืมเปลี่ยนชื่อ policy**
    
    ![Rename Policy](/images/IAM29.267212ae.png)
    
6.  คุณสามารถดูรายการ operations ที่ได้รับอนุญาตสำหรับ role นี้
    
    ![Operations List](/images/IAM9.ac63fdb0.png)
    
7.  คลิก **Next** เมื่อคุณพร้อมที่จะดำเนินการกำหนด scope
    
8.  คุณสามารถเลือกระหว่าง Full Access (ทุก entity types และ instances) หรือ Configure Access (เลือก entity types และ instances) เนื่องจากเรากำลัง filtering ตาม **User`##`:Production** โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย ให้ทำตามขั้นตอนต่อไปนี้ให้เสร็จสมบูรณ์:
    
    -   เลือก **Configure Access : Select entity types and instances**
    -   เลือก **All Entity types**
    -   เลือก **In Category** ในฟิลด์ Filters
    -   ค้นหา **User`##`:Production** โดยที่ `##` คือหมายเลขที่คุณได้รับมอบหมาย
    
    ![Select Entity Types](/images/IAM10.97559e0a.png)
    
9.  คลิก **Next** เมื่อคุณพร้อมที่จะดำเนินการ Assign Users
    
10.  เลือก **ntnxlab(AD)** และเลือก AD user **operator`##`@ntnxlab.local**
    
    ![Select AD and User](/images/IAM11.48c78866.png)
    
11.  คลิก **Save** คุณจะเห็นข้อความ "Policy created successfully" เมื่อทำสำเร็จ
    

## Authorization Policy Verification

1.  Sign Out ผู้ใช้ปัจจุบัน (current user) ออกจาก Prism Central โดยคลิกที่ `Adminuser##` ที่มุมบนขวาและคลิก **Sign Out**
    
2.  Login เข้าสู่ Prism Central ด้วย **operator`##`** ที่เพิ่งได้รับการอัปเดต โดยใช้ PC password จากหน้า Connection Details
    
    !!! tip
        ตรวจสอบให้แน่ใจว่าข้อความ **Login with your Company ID** แสดงอยู่บนหน้าจอ login
    
3.  หาก navigation bar ยังไม่ถูกขยาย ให้คลิกที่ไอคอนแฮมเบอร์เกอร์ (hamburger icon) ที่ด้านซ้ายบน
    
4.  ไปที่ **Compute & Storage > VMs**
    
    ![Navigation to VMs](/images/IAM12.f0d474b5.png)
    
5.  จะมีเพียง VMs ที่เป็นของ Category **User`##`:Production** เท่านั้นที่ควรจะแสดงขึ้นที่นี่ ขอบคุณ authorization policy ที่เราทำ configured เอาไว้
    
    ![List of VMs](/images/IAM14.3a0b7360.png)
    
6.  มาลองเลือก VM แล้วคลิก **Actions**
    
7.  เราจะเห็นได้ว่า user สามารถดำเนินการ (perform) ได้เฉพาะ limited operations เช่น Update, Launch Console และ Power Operations เท่านั้น
    
    ![Limited Actions](/images/IAM15.71a19856.png)
