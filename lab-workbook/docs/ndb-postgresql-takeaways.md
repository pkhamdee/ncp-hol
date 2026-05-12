# Takeaways

🎉 ขอแสดงความยินดีที่คุณสำเร็จหลักสูตร bootcamp นี้! คุณได้สัมผัสกับ database lifecycle management และได้เห็นความสามารถบางส่วนของ NDB เช่น:

-   การกู้คืน databases อย่างรวดเร็ว (Recovering databases quickly)
    
    -   NDB ใช้ประโยชน์จากความสามารถในการทำ snapshot ที่มีมาให้ใน Nutanix Cloud Platform เพื่อเปิดใช้การกู้คืน database โดยใช้เวลาหยุดทำงาน (downtime) น้อยที่สุด
-   การทำ On Demand และ Scheduled Patching
    
    -   กระบวนการ patching ของ NDB จะช่วยให้คุณรักษา databases ของคุณให้ทันสมัย (up to date) อยู่เสมอ ซึ่งทำให้มีความปลอดภัยมากขึ้นและมีความเสี่ยงต่อการถูกโจมตีน้อยลง
-   การทำ Copy Database Managment
    
    -   clones ของ NDB มีความบาง (thin) และประหยัดพื้นที่ (space-efficient) ซึ่งหมายความว่า database ขนาด 20TB จะไม่กินพื้นที่เพิ่มอีก 20TB; โดยมันจะติดตาม (track) เฉพาะความแตกต่างที่เกิดจาก source database เท่านั้น สิ่งนี้ช่วยให้สามารถสร้างและรีเฟรช (refreshing) ตัว clones ได้อย่างรวดเร็ว

## Additional Resources

-   [NDB Test Drive](https://cloud.nutanixtestdrive.com/login?source=one-platform&type=ndb&lpurl=one-platform-ndb?utm_source=nutanixbible&utm_medium=referral)
    
-   [NDB YouTube Playlist](https://youtube.com/playlist?list=PLAHgaS9IrJeeaY9GizEOtREfbyVIYUCKg&si=mDfKvKZ8qKg0Lnxt)
    
-   [Database on Nutanix Validated Design](https://portal.nutanix.com/page/documents/solutions/details?targetId=NVD-2155-Nutanix-Databases:NVD-2155-Nutanix-Databases)
    
-   [NDB on the Nutanix Cloud Bible](https://www.nutanixbible.com/22-book-of-ndb.html)


---

[← Back: Create and Manage Clones](ndb-postgresql-create-manage.md) | [Home](ndb-getting-started.md) | [Next: NDB REST API Explorer →](ndb-postgresql-api.md)