# File Blocking

## Selective File Blocking

ในแบบฝึกหัดนี้ คุณจะได้กำหนดค่า Files เพื่อบล็อก file extensions เฉพาะสำหรับ file server และ Marketing share

1.  ในแท็บเบราว์เซอร์ Nutanix Files ของคุณ ให้นำทางไปยัง **Configuration > Blocked File Types**
    
2.  ในช่อง _Blocked File Types_ ให้ป้อนรายชื่อ extensions ที่คั่นด้วยเครื่องหมายจุลภาค (comma) ว่า `.flv, .mov` แล้วคลิก **Save**
    
    ![](/images/1.44c3e313.png)
    
3.  ใน Windows Tools VM ของคุณ ให้เปิดหน้าต่าง PowerShell โดยคลิกที่ **ไอคอน PowerShell** บนทาสก์บาร์ ป้อนคำสั่งต่อไปนี้ ตรวจสอบให้แน่ใจว่าคุณได้แก้ไข `User##` ให้ตรงกับของคุณ
    
    ```
    new-item \\BootcampFS.ntnxlab.local\`User##`-marketing\MyMovie.flv -ItemType file
    ```
    
    รูปภาพนี้แสดงให้เห็นก่อนและหลังจากการระบุ blocked file extensions
    
    ![](/images/2.615ea45d.png)
    
4.  กลับไปที่แท็บเบราว์เซอร์ Files ของคุณ และนำทางไปยัง **Shares > `User##`\-Marketing**
    
5.  คลิก **Update > Next**
    
6.  ทำเครื่องหมายที่ **Blocked File Types** และป้อน `.none` เป็น extension
    
    ![](/images/3.5a338186.png)
    
7.  คลิก **Next > Update** เพื่อเสร็จสิ้นการอัปเดต
    
8.  กลับไปที่ PowerShell และดำเนินการคำสั่งจากขั้นตอนที่ 3 อีกครั้ง ตอนนี้คำสั่งจะทำงานสำเร็จ
    
    ![](/images/4.5daf96ad.png)
    
    การตั้งค่า Blocked file type ที่ระบุในระดับ share จะเขียนทับ (override) การตั้งค่าที่ระบุในระดับ server
    
9.  คลิก < ที่มุมซ้ายบนเมื่อคุณพร้อมที่จะดำเนินการในส่วนถัดไป