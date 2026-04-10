# Deploying Workloads

## Overview

ถึงตอนนี้เราได้ดู storage และ network provisioning แล้ว มาดูกันว่า workload สามารถ deploy ได้อย่างไร การสร้าง VM, การจัดการ, และการตรวจสอบสามารถทำได้สำหรับ Nutanix AHV โดยตรงผ่าน Prism Central Prism ยังรองรับการดำเนินการ VM CRUD (create, read, update, delete) สำหรับคลัสเตอร์ Nutanix ที่รัน ESXi ด้วยเช่นกัน

ในแบบฝึกหัดต่อไปนี้ เราจะสร้าง VM จาก source media และ disk image ที่มีอยู่

## Creating a Windows VM

ตอนนี้คุณจะสร้าง Windows Server VM จาก Windows installation ISO image

AHV มีฟีเจอร์ Image Service เพื่อสร้าง store ของไฟล์ ISO หรือ disk image ที่นำเข้า Image Service รองรับรูปแบบ disk ได้แก่ raw, vhd, vhdx, vmdk, vdi, iso, และ qcow2 มาดู image repository กัน

1.  จากเมนูด้านข้าง เลือก **Compute** และในส่วนนั้นคลิก **Images**
    
    ![](images/image_list.c6679b05.png)
    
2.  คุณจะเห็นรายการ image ที่อัปโหลดแล้วไปยังคลัสเตอร์นี้ เราจะใช้บางส่วนเพื่อ deploy VM ในขั้นตอนถัดไป
    
    ![](images/image_list2.cf1d5c33.png)
    

Note

คุณสามารถไปที่หน้านี้ได้โดยค้นหา **Images** ในช่องค้นหา

เพื่อให้ IO ประสิทธิภาพสูงแก่ VM AHV ต้องติดตั้ง paravirtualized driver เข้าใน guest (คล้ายกับ VMware Tools) สำหรับ Windows guest โดยเฉพาะ driver เหล่านี้ต้องถูกโหลดระหว่างการติดตั้งเพื่อให้ disk ของ VM สามารถเข้าถึงได้โดย Windows installer

Nutanix ตรวจสอบและจัดจำหน่าย driver เหล่านี้ผ่าน [Nutanix Portalopen in new window](http://portal.nutanix.com) ISO image ที่มี driver ถูกอัปโหลดไปยัง Image Service สำหรับ bootcamp นี้แล้ว

1.  จากเมนูด้านข้าง เลือก **Compute** และในส่วนนั้นคลิก **VMs**
    
    ![](images/deploy_workload1.ae80d1d2.png)
    
2.  คลิกปุ่ม **Create VM** เพื่อเปิด wizard สร้าง VM
    
    ![](images/deploy_workload2.188d5f39.png)
    
3.  กรอกข้อมูลในช่องต่อไปนี้ในหน้า configuration และคลิก **Next**:
    
    ทิ้งการตั้งค่าอื่นๆ ไว้ที่ค่าเริ่มต้น
    
    -   **Name** - `Initials`\-winvm
    -   **vCPU(s)** - `2`
    -   **Cores per CPU** - `1`
    -   **Memory** - `4` GiB
    
    ![](images/deploy_workload3.d1a93c9e.png)
    
    Note
    
    ภายใต้ Advanced Settings คุณจะเห็นตัวเลือกสำหรับการเปิดใช้งาน Memory Overcommit และการเปิดใช้งาน advanced processor compatibility (APC) ซึ่งช่วยให้ VM ย้ายระหว่างโฮสต์ที่มี CPU generation ต่างกันหากจำเป็น ด้านล่างเป็นตัวอย่างของสิ่งที่มันดูเมื่อคุณต้องการเปิดใช้งาน APC ซึ่งมี CPU generation พื้นฐานที่ mapping ไว้แต่คุณสามารถเลือก generation เฉพาะตามต้องการได้
    
    ![](images/apc.c90e5e90.png)
    
4.  คลิกปุ่ม **Attach Disk**
    
    ![](images/deploy_workload4.28843157.png)
    
5.  เลือกตัวเลือกต่อไปนี้และคลิก **Save**
    
    -   **Type** - CD-ROM
    -   **Operation** - Clone from Image
    -   **Image** - Windows2019.iso
    -   **Bus Type** - SATA
    
    การดำเนินการนี้จะ mount Windows Server ISO จาก Image Service เพื่อ boot/ติดตั้ง
    
    Note
    
    AHV รองรับทั้ง SATA และ IDE bus type สำหรับกรณีการใช้งานส่วนใหญ่ SATA คือ bus type ที่ใช้สำหรับ CD-ROM ควรใช้ IDE เฉพาะเมื่อต้องการความเข้ากันได้เฉพาะเช่นในกรณีของ OS เก่า IDE CD-ROM ไม่รองรับสำหรับ UEFI boot
    
    ![](images/deploy_workload5.52c61cdb.png)
    
6.  คลิก **Attach Disk**
    
    ![](images/deploy_workload6.b3c3264f.png)
    
7.  เลือกตัวเลือกต่อไปนี้และคลิก **Save**
    
    -   **Type** - Disk
    -   **Operation** - Allocate on Storage Container
    -   **Storage Container** - `Initials`\-Container
    -   **Capacity** - 50 GiB
    -   **Bus Type** - SCSI
    
    ![](images/deploy_workload7.6c72654c.png)
    
    การดำเนินการนี้จะสร้าง vDisk ว่าง 50GiB บน Storage Container ที่เลือกซึ่งเราจะติดตั้ง OS
    
8.  คลิก **Attach Disk** อีกครั้ง
    
9.  เลือกตัวเลือกต่อไปนี้และคลิก **Save**
    
    -   **Type** - CD-ROM
    -   **Operation** - Clone from Image
    -   **Image** - Nutanix-VirtIO-x.x.x.iso
    -   **Bus Type** - SATA
    
    การดำเนินการนี้ attach VirtIO iso ซึ่งจะติดตั้ง VirtIO driver ที่จำเป็นสำหรับ VM
    
    ![](images/deploy_workload8.adbba3b2.png)
    
10.  ต่อไปมาสร้าง vNIC สำหรับ VM คลิก **Attach to Subnet** เพื่อเชื่อมต่อ VM กับเครือข่าย
    

![](images/deploy_workload9.c5f936eb.png)

11.  เลือกตัวเลือกต่อไปนี้และคลิก **Save**

-   **Subnet** - `Initials`\-net
-   **Network Connection State** - Connected
-   **Attachment Type** - Access

![](images/deploy_workload10.96a0b640.png)

การดำเนินการนี้จะเพิ่ม virtual NIC เดียวให้กับ VM บน Virtual Network ที่เลือก

12.  สำหรับการกำหนดค่า boot เลือก **Legacy BIOS Mode** และคลิก **Next** หาก pop-up ปรากฏขึ้นให้ยืนยัน yes

Note

AHV รองรับ UEFI BIOS mode ซึ่งช่วยให้คุณใช้ Secure Boot กับ Windows Credential Guard รวมถึง vTPM

![](images/deploy_workload11.f9b59038.png)

13.  ในหน้า Management เราสามารถ customize VM ที่นี่เราจะใช้ **Default Storage Policy** กับ VM นี้ซึ่งจะกำหนด category ที่เกี่ยวข้องให้โดยอัตโนมัติ ผู้ดูแลระบบยังสามารถกำหนด custom category และค่าให้กับ VM ที่พวกเขาสร้างซึ่งจะกำหนด policy ที่เกี่ยวข้อง ตัวอย่างเช่น protection policy หรือ security policy ที่ถูกใช้กับ VM เมื่อถูกกำหนด category และค่าที่สอดคล้อง

-   สลับปุ่ม Enable Default Storage Policy เพื่อใช้งาน
-   เปลี่ยน Timezone เป็น Local Timezone
-   คงค่าที่เหลือเป็นค่าเริ่มต้น
-   คลิก **Next**

![](images/deploy_workload12.0e86b9d8.png)

14.  ในหน้า **Review** คุณสามารถตรวจสอบการตั้งค่าและการกำหนดค่าที่เลือกสำหรับ VM และแก้ไขหากจำเป็น คลิก **Create VM** เพื่อสร้าง VM

![](images/deploy_workload13.e8606b31.png)

15.  เมื่อ VM ปรากฏในรายการ คลิกขวาที่ VM และคลิก **Power On** เพื่อเปิดเครื่อง VM

![](images/deploy_workload14.38b6a489.png)

16.  เมื่อ VM มีจุดสีเขียวถัดจากชื่อ คุณสามารถคลิกขวาอีกครั้งและคลิก **Launch Console** เพื่อติดตั้ง OS บน VM

![](images/deploy_workload15.989b7edf.png)

17.  ดำเนินการผ่านคำถามการติดตั้งมาตรฐานจนถึงตำแหน่งการติดตั้ง Windows เมื่อได้รับแจ้งให้เลือก

-   **I don't have a product key**
-   **Windows Server 2019 Datacenter (Desktop Experience)**
-   **Custom : Install Windows Only** เมื่อได้รับตัวเลือก

18.  เนื่องจาก VM ไม่มี driver จึงไม่สามารถเห็น vdisk ที่ attach ได้ ดังนั้นก่อนอื่นเราจะโหลดและติดตั้ง driver คลิก **Load Driver** และไปที่ CD ที่ Nutanix VirtIO ISO ถูก mount และค้นหาไปยังโฟลเดอร์ที่สอดคล้องกับ OS

![](images/deploy_workload16.e139a5e2.png)![](images/deploy_workload17.9a5b4662.png)  
![](images/deploy_workload18.695dc132.png)

19.  เลือก Nutanix driver ทั้งหมดที่แสดง กด **Ctrl+a** เพื่อเลือก driver ทั้งหมดและคลิก **Next**

![](images/deploy_workload19.4d7fb1cb.png)

20.  หลังจากโหลด driver แล้ว vdisk ที่สร้างข้างต้นจะปรากฏเป็นเป้าหมายการติดตั้ง เลือกนั้นและคลิก **Next** เพื่อเสร็จสิ้นการติดตั้ง OS

![](images/deploy_workload20.1e749f1d.png)

Note

ถึงเวลาดื่มกาแฟ ☕ หรือเครื่องดื่มที่คุณชื่นชอบ คุณสามารถใช้เวลานี้สำรวจ Prism Central ด้วยหากต้องการ

21.  เมื่อการติดตั้งเสร็จสิ้น คุณสามารถ unmount ISO โดยใช้ปุ่ม **Unmount ISO** ใน console

### Installing Nutanix Guest Tools (NGT)

Nutanix Guest Tools ให้ความสามารถสำหรับเครื่องมือ backup ของ 3rd party บางตัวและการ replication ของ Nutanix เองเพื่อใช้ประโยชน์จาก VSS และดำเนินการต่างๆ เช่นการเปลี่ยน IP address ระหว่าง failover ไม่ใช่ข้อกำหนดสำหรับ VM ทุกตัวแต่ควรมีไว้ ต่อไปเราจะติดตั้ง NGT บน VM นี้

ก่อนหน้านี้คุณต้อง mount ISO สำหรับการติดตั้งนี้ด้วยแต่ตอนนี้ NGT สามารถ deploy ได้โดยตรงจาก Prism Central หากต้องการยังมีตัวเลือกในการดาวน์โหลด MSI installer แยกต่างหากและ deploy มัน

1.  ในการติดตั้ง NGT จาก Prism Central คลิกขวาที่ VM และวางเมาส์เหนือ **Guest Tools** แล้วเลือก **Set Up NGT** คลิก **Next** ในหน้า **Choose Preferences**
    
    ![](images/deploy_workload21.bb983bf4.png)
    
2.  เลือกกล่องสำหรับทั้ง **Volume Shadow Copy Services** และ **Self Service Restore** และคลิก **Next**
    
    ![](images/deploy_workload22.cf11a361.png)
    
3.  หากเราเลือกเครือข่ายที่สามารถ route ไปยัง VM ได้ ณ จุดนี้เราสามารถให้ชื่อผู้ใช้และรหัสผ่านสำหรับ VM และ Prism Central จะติดตั้ง NGT โดยอัตโนมัติ เนื่องจากเครือข่ายของเราไม่สามารถ route ได้ เราจะติดตั้งด้วยตนเอง คลิก **Mount Installer** เพื่อ mount NGT CD-ROM และเสร็จสิ้นการตั้งค่า
    
    ![](images/deploy_workload23.22fc261a.png)
    
4.  จาก VM console รัน NGT installer เพื่อติดตั้ง NGT
    
    ![](images/deploy_workload24.4399c619.png)
    
5.  เมื่อติดตั้ง NGT แล้ว ในหน้า VM มันจะแสดง OS ของ VM และเวอร์ชันของ NGT ที่ติดตั้ง ตอนนี้คุณมี VM ที่ทำงานและ deploy แล้วตัวแรกบน AHV 🎉 🎊
    
    ![](images/deploy_workload25.0ecf805f.png)
    

## Creating a Linux VM

ต่อไป ตอนนี้เราจะสร้าง Linux VM จาก disk image แบบ pre-installed ที่มีอยู่ใน Image Service เป็นเรื่องปกติในหลายสภาพแวดล้อมที่มี template-style image ของระบบปฏิบัติการที่ติดตั้งไว้ล่วงหน้า คล้ายกับแบบฝึกหัดก่อนหน้า disk image ถูกอัปโหลดไปยัง Image Service แล้ว

1.  คลิกปุ่ม **Create VM** เพื่อเปิด wizard สร้าง VM
    
    ![](images/deploy_workload2.188d5f39.png)
    
2.  กรอกข้อมูลในช่องต่อไปนี้และคลิก **Next**:
    
    ทิ้งการตั้งค่าอื่นๆ ไว้ที่ค่าเริ่มต้น
    
    -   **Name** - `Initials`\-linuxvm
    -   **vCPU(s)** - `2`
    -   **Cores per CPU** - `1`
    -   **Memory** - `4` GiB
    
    ![](images/deploy_workload_linux1.8e1a0197.png)
    
3.  คลิกปุ่ม **Attach Disk**
    
    ![](images/deploy_workload4.28843157.png)
    
4.  เลือกตัวเลือกต่อไปนี้และคลิก **Save**
    
    -   **Type** - Disk
    -   **Operation** - Clone from Image
    -   **Image** - CentOS7.qcow2
    -   **Capacity** - 20 GiB
    -   **Bus Type** - SCSI
    
    ![](images/deploy_workload_linux2.a075817a.png)
    
5.  ต่อไปมาสร้าง vNIC สำหรับ VM คลิก **Attach to Subnet** เพื่อเชื่อมต่อ VM กับเครือข่าย
    
    ![](images/deploy_workload_linux3.c666260d.png)
    
6.  เลือกตัวเลือกต่อไปนี้และคลิก **Save**
    
    -   **Subnet** - `Initials`\-net
    -   **Network Connection State** - Connected
    -   **Attachment Type** - Access
    
    ![](images/deploy_workload10.96a0b640.png)
    
    การดำเนินการนี้จะเพิ่ม virtual NIC เดียวให้กับ VM บน Virtual Network ที่เลือก
    
7.  สำหรับการกำหนดค่า boot เลือก **Legacy BIOS Mode** และคลิก **Next** หาก pop-up ปรากฏขึ้นให้ยืนยัน yes
    
    Note
    
    AHV รองรับ UEFI BIOS mode ซึ่งช่วยให้คุณใช้ Secure Boot กับ Windows Credential Guard รวมถึง vTPM
    
    ![](images/deploy_workload11.f9b59038.png)
    
8.  ในหน้า Management สำหรับ VM นี้เราจะไม่ใช้ default storage policy คงค่าเริ่มต้นและคลิก **Next**
    
    ![](images/deploy_workload_linux4.d82a662c.png)
    
9.  ในหน้า review ตรวจสอบการเลือกและคลิก **Create VM** เพื่อสร้าง Linux VM ของเรา
    

![](images/deploy_workload_linux5.4d98fc2f.png)

11.  หากต้องการ เปิดเครื่อง VM ที่สร้างและเชื่อมต่อกับ Console เพื่อดู Linux Virtual Machine ใหม่

## Takeaways

-   ในแล็บนี้ คุณเห็นว่าการ deploy ทั้ง Windows และ Linux VM เป็นเรื่องง่ายเพียงใด
-   เครื่องมือ Image Configuration ช่วยให้คุณจัดทำรายการ image ที่มีอยู่ที่ใช้ใน VM deployment ตามต้องการและครอบคลุมรูปแบบที่หลากหลายรวมถึง raw, vhd, vhdx, vmdk, vdi, iso, และ qcow2
