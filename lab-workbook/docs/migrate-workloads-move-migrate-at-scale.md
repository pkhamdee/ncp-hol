# Migrate at Scale with Nutanix Move

ตอนนี้คุณได้ทำการ basic migration ของ VM และการใช้งาน advanced features บางส่วนภายใน Nutanix Move แล้ว สิ่งนี้ได้เตรียมความพร้อมให้คุณสำหรับคำถามและสถานการณ์ต่างๆ ที่มักพบบ่อยในการทำ migrations ในส่วนนี้เราจะมาสำรวจกันว่าการใช้งาน Move สำหรับ large scale migrations จะเป็นอย่างไร

ด้วยความรู้ที่คุณได้เรียนรู้มาแล้วและประสบการณ์ที่ผ่านมาของคุณ คุณจะได้รับคำแนะนำเกี่ยวกับวิธีการใช้ Move ไม่ว่าจะเป็นแค่เพียงไม่กี่ VMs หรือเป็นพันๆ ก็ตาม

1.  Nutanix Move ถูก deploy เป็น virtual appliance อย่างที่คุณได้ใช้งานไป Move appliance เครื่องเดียวสามารถเคลื่อนย้าย workloads ทั้งหมดสำหรับการ migrations ของ customers ส่วนใหญ่ได้
    
    -   มีบาง scenarios ที่องค์กรอาจจำเป็นต้องใช้ Move appliances หลายตัว
    -   หากมีปัญหาเรื่อง network access หรือ segmentation ที่ขัดขวางไม่ให้ appliance เพียงตัวเดียวสามารถเชื่อมต่อกับ sources และ targets ที่ต้องการได้ทั้งหมด สิ่งนี้อาจจำเป็นต้องมี appliances หลายตัวไปวางอยู่ใน network zones ที่แตกต่างกัน

2.  องค์กรควรใช้ migration plans กี่อันภายใน Move appliance?
    
    -   มากเท่าที่ customer ต้องการหรือจำเป็นตามขนาด ความซับซ้อน และ change windows ที่มีอยู่ เป็นต้น
    -   จะมีความสมดุลที่สมเหตุสมผลระหว่างการมีกลุ่มขนาดใหญ่เพียงไม่กี่กลุ่ม กับการมี plan สำหรับแต่ละ individual VM
    -   มักจะสมเหตุสมผลสำหรับ tiers และส่วนต่างๆ ของ application ที่จะถูก moved ไปพร้อมกันในส่วนใหญ่ ดังนั้นการจัดกลุ่ม app, web, และ data tiers เข้าด้วยกันและใช้แค่ single change window ก็น่าจะสมเหตุสมผลสำหรับคนส่วนมาก ขึ้นอยู่กับ scale สิ่งนี้สามารถทำได้ด้วย single plan หรืออาจจำเป็นต้องใช้ multiple plans
    
3.  หาก organizations กำลังสร้างและใช้งาน large migration plans นั่นหมายความว่าต้อง cutover ไปพร้อมกันหมดหรือไม่?
    
    -   ไม่ แม้ว่า entire plan จะสามารถ failed over ไปพร้อมกันทั้งหมดได้ แต่ Move อนุญาตให้สามารถ cutover ได้ตั้งแต่ VM ตัวเดียวที่อยู่ใน plan ไปจนถึง groups of VMs

4.  เพื่อยกตัวอย่าง scenarios เหล่านี้ เราได้สร้าง click-through demo สำหรับ lab ในส่วนนี้ คลิกที่นี่เพื่อเปิด [Move at Scale Demo](https://nutanix.storylane.io/share/tvfi7lhuodu3?utm=holmigrate) กลับมาที่ lab guide นี้เมื่อคุณทำ demo เสร็จแล้ว
    

🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉

ขอแสดงความยินดีด้วย!! คุณเพิ่งเสร็จสิ้นการ migrating at scale

ต่อไป มาสำรวจการใช้ Move เพื่อทำการ migrate file shares กัน


---

[← Back: Advanced VM Migrations](migrate-workloads-move-advanced-migration.md) | [Home](migrate-nutanix-overview.md) | [Next: Migrating Shares with Move →](migrate-workloads-move-share.md)