# Takeaways

-   Nutanix Move เป็นโซลูชัน cross-hypervisor mobility สำหรับการย้าย VMs โดยมี downtime น้อยที่สุด
-   Nutanix Move รองรับ mobility จากหลากหลาย sources และ destinations รวมถึง ESXi, Hyper-V, AWS, Azure และ AHV สำหรับรายชื่อการ migrations ที่รองรับทั้งหมด โปรดดูที่ [user guideopen in new window](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Move-v5_5:Nutanix-Move-v5_5)
-   ปัจจุบัน Nutanix Move ยังรองรับการ migrating ตัว file shares จาก non-Nutanix shares ไปยัง Nutanix Files ด้วย
-   การทำ Advanced migration ด้วย Move สามารถ filter และ apply ตัว categories เพื่อนำเข้าสู่ policies ได้ในทันที (immediate placement)
    -   Prism Central policies เช่น Security Policies, Protection Policies, Storage Policies และอื่นๆ สามารถ setup ไว้ล่วงหน้าเพื่อเริ่มต้นการปกป้อง (protecting) VMs ได้ทันทีเมื่อพวกมันถูก migrated
-   การมี Multiple migration plans ช่วยให้การทำ large migrations เป็นไปได้
-   Playbooks ช่วยให้การทำ automation สำหรับ event notification เป็นเรื่องง่าย

## Resources

-   [Move User Guideopen in new window](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Move-v5_5:Nutanix-Move-v5_5)
-   [Nutanix Files User Guideopen in new window](https://portal.nutanix.com/page/documents/details?targetId=Files-v5_1:Files-v5_1)
