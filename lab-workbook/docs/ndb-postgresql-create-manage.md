# Create and Manage Clones

คุณสามารถสร้าง clone ได้อย่างง่ายดายภายใน NDB โดยใช้ Time Machine ซึ่ง Database clones นั้นมีประโยชน์สำหรับวัตถุประสงค์ในการ development และ testing ช่วยให้ environment ที่ไม่ใช่ production สามารถใช้ข้อมูล production ได้โดยไม่ส่งผลกระทบต่อการทำงานของ production โดย NDB clones ใช้เทคโนโลยีการทำ clone แบบ Nutanix-native copy-on-write ซึ่งทำให้สามารถสร้าง zero-byte database clones ได้ ประสิทธิภาพด้านพื้นที่นี้ช่วยลดต้นทุน storage สำหรับ environment ที่รองรับ database clones ขนาดใหญ่ได้อย่างมาก โดยการลดความจุของ storage ที่ต้องการลงอย่างมหาศาล

## Clone the Fiesta Database

ในฐานะ DBA คุณได้รับมอบหมายให้สร้าง clone ของ Production inventory database เพื่อนำไปใช้โดยทีม development โดยจะต้องมีการ refresh ทุกสัปดาห์ภายในเวลา 6:00 น. ของวันปัจจุบัน มาดูขั้นตอนในการสร้าง clone นั้นกัน

1.  ภายใน NDB ให้เลือก **\> Data Protection > Time Machines** จากเมนู drop-down จากนั้นเลือก **List** จากเมนูด้านบน ต่อมาให้คลิกที่จุดข้างๆ รายการ `User##`\-fiestadb\_TM ของคุณ
    
    ![](/images/n01.76184e9b.png)
    
2.  คลิก **Actions > Create a Clone of the PostgreSQL Instance**
    
3.  ในส่วนของ _Time/Snapshot_ ให้เลือก **Snapshot** จากนั้นเลือก snapshot ที่มีอยู่
    
    ![](/images/n02.ec5c4ccc.png)
    
    !!! note
        -   แม้จะไม่ได้สร้าง manual snapshots ทาง NDB ก็ยังมีความสามารถในการ clone database ตาม increments แบบ point-in-time ซึ่งรวมถึง Continuous (ทุกวินาที), Daily, Weekly, Monthly, หรือ Quarterly โดยที่ SLA จะควบคุม availability
        -   คุณอาจเห็นสีแดงบน schedule graph หาก database ของคุณยังคงอยู่ในช่วง recovering จากแบบฝึกหัดก่อนหน้านี้
    
4.  คลิก **Next**
    
5.  ในส่วนของ _Database Server VM_ ให้กรอกข้อมูลในช่องต่อไปนี้ แล้วคลิก **Next**
    
    -   **Database Server VM** - Create New Server
    -   **Database Server VM Name** - `User##`\-postgres\_dev
    -   **Compute Profile** - CUSTOM\_EXTRA\_SMALL
    -   **Network Profile** - DEFAULT\_OOB\_POSTGRESQL\_NETWORK
    -   **SSH Public Key** - เลือก **Text** จากนั้น copy/paste โค้ดด้านล่างลงใน text field
    
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoQRdEfm8ZJNGlYLQ2iw08eVk/Wyj0zl3M5KyqKmBTpUaS1uxj0K05HMHaUNP+AeJ63Qa2hI1RJHBJOnV7Dx28/yN7ymQpvO1jWejv/AT/yasC9ayiIT1rCrpHvEDXH9ee0NZ3Dtv91R+8kDEQaUfJLYa5X97+jPMVFC7fWK5PqZRzx+N0bh1izSf8PW0snk3t13DYovHFtlTpzVaYRec/XfgHF9j0032vQDK3svfQqCVzT02NXeEyksLbRfGJwl3UsA1ujQdPgalil0RyyWzCMIabVofz+Czq4zFDFjX+ZPQKZr94/h/6RMBRyWFY5CsUVvw8f+Rq6kW+VTYMvvkv
    ```
    
    ![](/images/n03.042e2942.png)
    
6.  ในส่วนของ _Instance_ ให้กรอกข้อมูลในช่องต่อไปนี้
    
    -   **Name** - `User##`\-fiestadb\_dev
    -   **POSTGRES Password** - `nutanix/4u`
    -   **Database Parameter Profile** - DEFAULT\_POSTGRES\_PARAMS
    
    ![](/images/n04.24ba1b38.png)
    
7.  เนื่องจากมีการร้องขอให้ clone นี้ทำการ refreshed จาก production ทุกสัปดาห์โดยเริ่มตั้งแต่วันนี้ ให้ทำเครื่องหมายเลือกที่ **Schedule Data Refresh** และกรอกข้อมูลในช่องต่อไปนี้
    
    -   **Refresh Every (days)** 7
    -   **Refresh Time** 00:06:00
    
    ![](/images/n05.4d000795.png)
    
    เมื่อกรอกข้อมูลในช่องด้านบนเรียบร้อยแล้ว ให้คลิก **Clone**
    
    !!! note
        -   หาก database clone นี้จำเป็นสำหรับช่วงเวลาใดเวลาหนึ่งเท่านั้น คุณสามารถเลือก Removal Schedule เพื่อตั้งวันที่ให้ทำการลบ database clone ออกโดยอัตโนมัติได้
        -   คุณสามารถตั้งค่าหรือเปลี่ยนแปลง Schedule Data Refresh หรือ Removal Schedule ใน Time Machine ของ database ได้ หากไม่ได้เลือกไว้ในขั้นตอน clone creation
        -   ส่วนของ Pre-Post Commands สามารถใช้ในการรัน scripts เพื่อเพิ่มหรือลบ users และ/หรือทำการ sanitize ตัว database ได้
    
8.  คุณสามารถตรวจสอบความคืบหน้าของ clone creation ได้โดยคลิกที่กล่องสีน้ำเงินที่มีคำว่า **click here**
    
    ![](/images/n06.504c38b1.png)
    

เมื่อคุณตรวจสอบแล้วว่าขั้นตอน Clone creation ได้เริ่มต้นขึ้น เราสามารถดำเนินการต่อในส่วนของ [Refresh Cloned Database](#refresh-cloned-database) ได้เลย

## Refresh Cloned Database

ความสามารถในการ refresh สิ่งที่เรียกกันว่า cloned database อย่างง่ายดาย โดยใช้ข้อมูลที่อัปเดตล่าสุดจาก source database ช่วยปรับปรุงกระบวนการ development, testing, และ use cases อื่นๆ ได้อย่างมาก โดยช่วยให้มั่นใจได้ว่าบุคคลเหล่านี้จะสามารถเข้าถึงข้อมูลล่าสุด คุณได้รับคำขอให้ refresh database จาก source; เรามาดำเนินการคำขอนั้นให้เสร็จสมบูรณ์กันเถอะ

1.  เลือก **\> Databases** จากนั้นเลือก **Clones** จากเมนูด้านบน
    
2.  เลือก `User##-fiestadb_clone` ของคุณ และคลิก **Refresh**
    
    ![](/images/n07.dac95e65.png)
    
3.  _Point in Time_ ล่าสุดที่มีอยู่จะถูกเลือกไว้เป็นค่าเริ่มต้น (default) คลิก **Refresh**
    
    !!! tip
        หาก point in time snapshot ไม่มีให้ใช้งาน ให้เลือก snapshot แล้วเลือก snapshot จาก drop down (ซึ่งอาจเกิดจากช่วงเวลาที่ lab environment ถูกสร้างขึ้น)
    
    ![](/images/n08.533f2e3b.png)
    
4.  เลือก **\> Operations** จากเมนูเพื่อทำการ monitor กระบวนการ _Refresh Clone_
    

ในหน้า operations page คุณควรจะเห็นสถานะของ clone-creation task จากด้านบนและ refresh clone task โดย tasks เหล่านี้จะเสร็จสมบูรณ์อย่างรวดเร็ว เนื่องจาก NDB ใช้ built-in snapshots ของ Nutanix Cloud Platforms ซึ่งเป็นแบบ thin และ space-efficient สิ่งนี้ช่วยให้สามารถทำ creation และ refreshing ของ clones ได้อย่างรวดเร็ว

## Takeaways

NDB:

-   รองรับ one-click operations สำหรับ registering, provisioning, cloning, และ refreshing ของ supported databases
-   ทำให้เกิดความเรียบง่ายและ operating efficiency ในรูปแบบเดียวกับที่คุณคาดหวังจาก public cloud provider ในขณะที่ยังคงให้ DBAs สามารถรักษาการควบคุม (maintain control) เอาไว้ได้
-   ทำ automate ให้กับ complex database operations ซึ่งช่วยลดเวลาที่ DBA ต้องใช้ รวมไปถึงลดค่าใช้จ่ายในการจัดการ databases ด้วย traditional technologies (Opex savings)
-   ให้อำนาจแก่ DBAs ในการสร้าง standardize ให้กับการทำ database deployments ของพวกเขาในหลากหลาย database engines ในขณะที่ผสมผสาน best practices ของแต่ละ database เข้าด้วยกันโดยอัตโนมัติ
-   ช่วยให้ DBAs สามารถ clone environments ของตนเองไปยัง application-consistent transaction ล่าสุดได้


---

[← Back: Database Provisioning and Patching](ndb-postgresql-provision.md) | [Home](ndb-getting-started.md) | [Next: Takeaways →](ndb-postgresql-takeaways.md)