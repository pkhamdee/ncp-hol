# Network Controller (Flow Controller in 7.5)

Flow Network Security Next-Gen ทำงานบนพื้นฐานของโปรเจกต์ OVN หรือ Open Virtual Networking โดยที่ Flow Controller (ก่อนหน้านี้คือ Network Controller, และก่อนหน้านั้นคือ Advanced Network Controller) ภายใน Prism Central ทำหน้าที่เป็น control plane สำหรับแจกจ่าย firewall rules สำหรับการทำ microsegmentation รวมถึงการแจกจ่าย centralized IPAM สำหรับ managed VLANs และทำงานในรูปแบบ VPCs (Virtual Private Clouds)

!!! note

control plane สำหรับ Nutanix networking ได้รับการพัฒนามาอย่างต่อเนื่อง ดังนั้นคุณอาจเห็นชื่อที่แตกต่างกันไปตามเวอร์ชันที่คุณกำลังใช้งาน

เริ่มแรก มันถูกเปิดตัวในชื่อ **Advanced Networking** พร้อมกับ **Advanced Network Controller** หรือ **ANC**

ต่อมา ได้เปลี่ยนชื่อเป็น **Network Controller**

**Flow Controller** ตัวใหม่ใน NCI 7.5 release ได้รวบรวม Flow Network Security, Flow Virtual Networking และ VLAN networking เข้าไว้ด้วยกันในที่เดียว

ใน lab environment นี้ `secondary` subnet ถูก setup ให้เป็น Flow Controller-backed VLAN คุณอาจเห็น "Basic" VLANs อื่นๆ ที่ไม่ถูก managed โดย Flow Controller เช่น `primary` โดยที่ basic VLANs เหล่านี้ไม่สามารถถูก protected โดย Flow Network Security Next-Gen ได้

lab นี้จะใช้ Flow Controller-backed VLAN subnets

Nutanix อ้างอิงถึง network entity ว่าเป็น **Subnet** ใน Prism interface

## Network Types

ก่อนอื่น มาดู labels ของ provisioned subnets ของเราใน Prism Central กัน

1.  ไปที่ **Infrastructure** > **Network & Security** > **Subnets**
    
2.  ตรวจสอบว่า **primary** subnet มีแท็ก **Basic** และ **Secondary** subnet ไม่มีแท็ก
    

![Subnet Types](/images/net-controller1.0333a7f4.png)

## Flow Controller Status

ถัดไป มาตรวจสอบว่า Flow Controller ถูก enabled หรือไม่ สิ่งนี้ได้ถูกทำไว้แล้วโดย default บน Large และ XLarge Prism Central deployments แต่คุณอาจต้อง enable สิ่งนี้แบบ manually หากคุณเลือกใช้ Prism Central ขนาดที่เล็กกว่า

1.  ไปที่ **Infrastructure** > **Prism Central Settings** > **Flow Management**
    
2.  ตรวจสอบว่า Flow Controller ถูก enabled, อยู่ใน healthy state, และรองรับ connected cluster สิ่งสำคัญคือต้องอ่าน resiliency recommendations ด้วย
    

![Flow Controller Status](/images/net-controller2.33dc8357.png)

1.  เราปล่อยให้ **Manage Default VLAN Settings** unchecked ไว้ ซึ่งหมายความว่า new subnets ทั้งหมดจะเป็น **Basic** subnets เว้นแต่ว่าจะมีการติ๊กช่องเพิ่มเติมเมื่อสร้างแต่ละ new subnet สำหรับการ deployments ของคุณเองใน production ให้พิจารณาติ๊กช่องนี้เพื่อประหยัดจำนวนคลิกในอนาคตหากคุณกำลังสร้าง new subnets แต่ให้ปล่อยเป็น unchecked ไว้ใน lab นี้

## Flow Network Security Status

ส่วนสุดท้ายของ configuration ของเราคือการตรวจสอบให้แน่ใจว่า Flow Network Security Next-Gen ถูก enabled เรียบร้อยแล้ว สิ่งนี้ได้ทำไว้แล้วใน lab cluster ของเรา แต่ก็เป็นเรื่องดีเสมอที่จะ verify!

ตรวจสอบว่า Flow Network Security `Next-Gen` แสดงผลเป็น **Enabled** ในส่วน capabilities ของ Flow Management

## Takeaways

Flow Network Security Next-Gen จำเป็นต้องใช้ Flow Controller และการ enable ตัว Flow Network Security คุณสามารถ protect ตัว VMs ที่อยู่ใน VLAN หรือ VPC subnets ได้ แต่ไม่สามารถทำได้ใน VLAN Basic subnets
