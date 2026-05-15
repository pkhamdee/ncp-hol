# Create a Recovery Plan

**Snapshots** ของเรากำลังถูกสร้างและทำการ **replicated** ตอนนี้เรามาจัดเตรียมทุกอย่างให้พร้อมเพื่อกู้คืนจาก **snapshots** เหล่านั้นใน **Cloud cluster** กันครับ

1. ใน **Core Prism Central** ให้เลือก **> Data Protection > Recovery Plans**

    !!! note

        หากคุณเป็นผู้ใช้คนแรกที่เข้ามาถึงส่วนนี้ของ **lab** คุณจะเห็นหน้าจอภาพรวมของ **Recovery Plan** ให้คลิก **Create new Recovery Plan** เพื่อออกจากหน้าจอนี้

2. ในส่วน **General** ที่ช่อง **Recovery Plan Name** ให้กรอก **RecoveryPlan-##** โดยที่ ## คือหมายเลขผู้ใช้งานของคุณ
    
3. คุณสามารถกรอก **Recovery Plan Description** ได้หากต้องการ แต่เป็นส่วนที่เลือกได้ (optional)
    
4. ภายใต้หัวข้อ **Locations** ให้คลิกรายการ **drop-down** ใต้ **Primary Location** และเลือก **Local AZ**
    
5. คลิกรายการ **drop-down** ใต้ **Recovery Location** และเลือก **Cloud Prism Central**
    
6. คลิก **Next**
    
7. ในหน้าจอ **Recovery Sequence** ให้คลิก **+ VM(s)** เพื่อเพิ่ม **VM** ของคุณเข้าใน **Recovery Plan**
    
8. เลือกช่อง **drop-down** และเลือก **Category**
    
    !!! note

        คุณสามารถใช้ **VM Name** เพื่อเพิ่มเข้าไปใน **Protection Policy** หรือ **Recovery Plan** ได้เช่นกัน แต่การใช้ **categories** เป็นแนวทางปฏิบัติที่ดีที่สุด (**best practice**) เนื่องจากมีความยืดหยุ่นกว่าและง่ายต่อการเปลี่ยนแปลงในภายหลังโดยไม่ต้องจัดการ **VMs** ทีละตัว

10. ในช่องค้นหาทางด้านขวาของ **Category** ให้พิมพ์ชื่อของ **category** ที่คุณสร้างไว้ คือ **DR-RPO-User##:1hr**
    
11. เลือกรายการที่ปรากฏในผลการค้นหาแล้วคลิก **Add**
    
    !!! note

        หลังจากเพิ่ม **VM** ของคุณแล้ว มันจะถูกจัดไว้ใน **VM Stage 1** โดยที่ **Boot Stages** คือส่วนที่คุณสามารถลำดับขั้นตอนการเปิดเครื่อง (**boot sequences**) ของ **VM** ได้ ซึ่งจะมีประโยชน์เมื่อต้องปกป้อง **application** เนื่องจากผู้ดูแลระบบสามารถกำหนดให้ **database server** เปิดเครื่องก่อน **application** และ **web servers** ได้

12. ภายใต้ **Network Type** ให้เลือก **"Stretch networks"**
    
    ![image](images/network_type.6bee8ba9.png)
    
13. ในแท็บ **Network Settings** ภายใต้หัวข้อ **Local AZ** (ฝั่งซ้าย) ให้คลิกรายการ **drop-down** ใต้ **Production Virtual Network or Port Group** และเลือก **stretch-net** สังเกตว่า **Prism** จะใส่ข้อมูลในช่อง **Gateway IP** และ **Prefix Length** ให้โดยอัตโนมัติ
    
14. เนื่องจากนี่เป็นสภาพแวดล้อมแบบ **lab** ในช่องรายการ **drop-down** ของ **Test Failback Virtual Network or Port Group** ให้เลือก **stretch-net** (ในสภาพแวดล้อมการทำงานจริงอาจมีเครือข่ายสำหรับทดสอบที่แตกต่างกันออกไป)
    
15. ภายใต้ส่วนของ **Cloud PC** (ฝั่งขวา) ให้เลือกเครือข่าย **stretch-net** เดียวกันในช่อง **Virtual Network or Port Group** ทั้งสองช่อง
    
16. คัดลอกค่า **Gateway IP** และ **Prefix Length** จากฝั่ง **Local AZ** มากรอกในช่องเหล่านั้นของฝั่ง **Cloud PC (Recovery)**
    
    ![image](images/network_mapping_complete.572e9589.png)
    
17. คลิก **Done**
    

## Next Steps

- **Cloud cluster** ของเราถูกตั้งค่าเรียบร้อยแล้ว
- **Subnet Extension** เปิดใช้งานจริงระหว่าง **Core** และ **Cloud** แล้ว
- **VM** มีการกำหนด **static IP**
- **VM's snapshots** กำลังถูก **replicated** ตามนโยบายจาก **Core** ไปยัง **Cloud**
- **Recovery plan** ถูกกำหนดไว้แล้ว เพื่อให้ **VM** ทราบวิธีการรันใน **Cloud**

ได้เวลาเคลื่อนย้ายไปสู่ **Cloud** แล้วครับ ⌚ 🚚 ☁️

[← Back: Create a Protection Policy](edge-lab-scenario3-protect.md) | [Home](edge-getting-started.md) | [Next: Failover →](edge-lab-scenario3-failover.md)