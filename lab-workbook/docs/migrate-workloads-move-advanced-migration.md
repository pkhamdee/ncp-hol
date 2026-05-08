# Advanced VM Migrations with Nutanix Move

ตอนนี้คุณได้ทำการ basic migration สำหรับ VM จาก source cluster ไปยัง destination cluster แล้ว ภายในโปรแกรม Move ยังมี features เพิ่มเติมที่ช่วยรับมือกับความท้าทายของการทำ advanced migrations ในส่วนนี้ของ lab จะมี pre-captured walk through สำหรับ advanced features บางส่วนเหล่านี้

คุณจะได้เห็น:

1.  การใช้ Tags และ Categories ในระบบ virtualization และ cloud เป็นวิธีการทำ management ที่ได้รับความนิยมและเติบโตมากขึ้น ด้วย unique labels เหล่านี้ที่ถูก apply ลงบน VMs คุณสามารถ control พฤติกรรมและ configuration ของ workload ได้มากมาย Nutanix Move มีความสามารถในการช่วยเหลือเรื่อง migration ของ workloads ที่ใช้ประโยชน์จาก Tags หรือ Categories ได้
    
    -   ใน Nutanix และโปรแกรม virtualization ของคู่แข่ง คุณสามารถ assign ตัว policies ผ่านทาง tag หรือ category ที่ถูก apply ไปยัง VM ได้
        
    -   การรวมสิ่งนี้เป็นส่วนหนึ่งของการ migration จะช่วยเพิ่ม VM security หลังจากการ migrate เนื่องจาก security policies ได้ถูกนำมาพิจารณาแล้ว นอกจากนี้ policies อื่นๆ เช่น backup, DR, storage ก็จะถูก apply ด้วย categories เหล่านี้เช่นกัน
        
2.  คุณยังจะได้สำรวจตัวเลือกในการใช้ Manual preparation option ใน advance migration plan Manual preparation ให้ความสามารถในการรัน Move preparation scripts จากภายนอก Move tool สำหรับ environments ที่มีมาตรการรักษาความปลอดภัยที่เข้มงวด มีเหตุผลสองสามข้อว่าทำไมถึงต้องใช้ Manual preparation mode แต่ที่พบบ่อยที่สุดคือเรื่องของ privileges ภายใน VM
    
    -   เป็นเรื่องปกติมากขึ้นที่องค์กรต่างๆ จะไม่แจกจ่าย elevated credentials ให้สำหรับกลุ่มของ VMs อย่างแพร่หลาย ซึ่งจำเป็นสำหรับการทำ Move automatic preparation
        
    -   อย่างไรก็ตาม หากคุณสามารถรัน scripts เหล่านี้ด้วยวิธีอื่นได้ คุณก็จะได้รับ Migration experience แบบเดียวกัน
        
    -   บ่อยครั้งองค์กรที่นำ strict security practices มาใช้ จะมี tool เพื่อรัน scripts จากระยะไกล (remotely execute) ด้วย unique credentials และสิ่งนี้น่าจะสามารถนำมาใช้งานร่วมกับ manual preparation scripts เหล่านี้ได้
        
3.  เพื่อยกตัวอย่าง scenarios เหล่านี้ เราได้สร้าง guided click-through demo สำหรับ lab ในส่วนนี้ คลิกที่นี่เพื่อเปิด [Advanced VM Migrations Demo](https://nutanix.storylane.io/share/r2ltquglgfpl?utm=holmigrate) กลับมาที่ lab guide นี้เมื่อคุณทำ demo เสร็จแล้ว
    

## When You're Done

🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉

ขอแสดงความยินดีด้วย!! คุณเพิ่งเสร็จสิ้นการ migration โดยใช้ advanced features บางอย่างใน Nutanix Move

ต่อไป มาสำรวจการใช้ Move เพื่อทำการ migrate at scale กัน
