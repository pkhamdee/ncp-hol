# Database Recovery

การทำ recovering a database อย่างรวดเร็วและมีประสิทธิภาพสามารถช่วยบริษัทของคุณประหยัดชั่วโมงทำงานและค่าใช้จ่ายได้ NDB ใช้ฟังก์ชัน snapshot แบบ built-in ของ Nutanix Cloud Platforms เพื่อให้บริการ quick recovery ของ databases รวมถึง datasets ที่มีขนาดใหญ่มาก

## Recovering the Source Database

ในฐานะ Database administrator (DBA) คุณได้รับแจ้งว่ามี database เกิดความเสียหาย (corrupted) และอยู่ในสถานะ offline เนื่องจากจำนวนของความเสียหาย คุณจะต้องทำการ recover แบบ whole database เรามาเริ่มกันเลยและดูว่า NDB จะสามารถช่วยคุณทำงานนี้ให้สำเร็จได้อย่างรวดเร็วแค่ไหน

1.  ภายใน NDB เลือก **\> Data Protection > Time Machines**
    
    นี่คือหน้า dashboard ของ **Time Machine** ใน panel แรกคุณสามารถดู health ของ Time Machines ของคุณและจำนวนของ databases ของคุณที่ได้รับการปกป้อง (protected) ไว้
    
    ![](/images/10.b0c62cbb.png)
    
    !!! note
        **Time Machine สามารถมีสถานะ (statuses) ดังต่อไปนี้**
        
        -   **Critical:** หาก backups กำลังล้มเหลว (failing) ในปัจจุบัน
        -   **Warning:** หาก Backups เคยล้มเหลวในอดีต แต่มี recent recovery พร้อมใช้งาน
        -   **Healthy:** หาก Backups ทั้งหมดมีสถานะ healthy
        -   **Paused:** หาก Time machine ถูก paused โดยลูกค้า, ถูก paused โดยระบบหลังจากที่เกิด log backup failures ติดต่อกัน 500 ครั้ง หรือหาก database ถูกลบไปแล้วแต่ตัว time machine ยังคงถูกเก็บรักษาไว้ (retained)
    
2.  ตอนนี้เลือก **List** จากเมนูด้านบน
    
    ![](/images/09.7692af78.png)
    
3.  เลือกรายการ `User##`\-fiestadb\_tm
    
    ![](/images/01.57ac5cd0.png)
    
4.  คลิก **\> Actions > Restore Source Instance**
    
    นี่คือหน้าจอ Time Machine recovery คุณสามารถทำการ recover ตัว database จาก point in time ใดก็ได้ที่ NDB มี snapshot เก็บไว้ เลื่อนลงมาที่ **Restore Data To** จากนั้นเลือก snapshot จาก dropdown แล้วคลิก **Restore**
    
    ![](/images/02.30cfc7ea.png)
    
    !!! note
        -   นอกเหนือจากใน lab environment คุณจะสามารถเลือกวันที่และนาทีที่แตกต่างกันเพื่อ restore ได้ ขึ้นอยู่กับ SLA ที่ตั้งค่าไว้สำหรับ database นั้น ซึ่งช่วยให้คุณสามารถ recover กลับไปก่อนที่ database จะเกิดความเสียหายได้
        -   คุณสามารถเลือกที่จะ back up ตัว logs ก่อนทำการ restore ได้ (tail log backup) แต่เราข้ามขั้นตอนนี้ไปเพื่อประหยัดเวลา
    
5.  ป้อนชื่อ database เพื่อยืนยันว่าคุณต้องการ overwrite สิ่งนี้ด้วย database อื่น จากนั้นคลิก **Restore** อีกครั้ง
    
    ![](/images/03.e64f8d3f.png)
    
6.  คลิกที่แถบสีน้ำเงินที่ด้านบนของหน้าจอเพื่อดูว่าการ restore ได้เริ่มต้นขึ้นแล้ว
    
    ![](/images/04.07c646ca.png)
    
    เมื่อคุณได้ยืนยันแล้วว่า recovery เริ่มต้นขึ้น ให้ดำเนินการต่อในส่วนของ [Recovering Data](#recovering-data)
    
    !!! note
        เนื่องจากเราเลือกที่จะข้ามการทำ log catch-up ก่อนเริ่ม recovery คุณอาจได้รับ alert ที่ระบุว่า Time Machine สำหรับ user database ของคุณนั้น unhealthy เพื่อแก้ไขปัญหานี้ ให้ไปที่ **Data Protection > Time Machines** แล้วเลือก **List** เลือก **`User##`-fiestadb_tm** ของคุณแล้วไปที่ **Actions > Snapshot** การดำเนินการนี้จะทำการ restore ตัว Time Machine ให้กลับมาอยู่ในสถานะ healthy เนื่องจากจำเป็นต้องสร้าง full snapshot ใหม่หรือทำการ log catchup ตามหลัง complete recovery โดยเฉพาะอย่างยิ่งหากคุณไม่ได้ดำเนินการทำ tail log backup
    

## Recovering Data

ในงานของคุณในฐานะ DBA ตอนนี้คุณได้รับคำขอให้ทำการ recover ตัว table และ database rows บางส่วน เช่นเดียวกับที่เราทำด้านบน การ recovery แบบ source database จะไม่สามารถใช้ได้ที่นี่ เนื่องจากพวกเขาไม่ต้องการสูญเสียข้อมูลอื่นๆ ที่ได้ป้อนไปแล้ว เราจำเป็นต้องทำการ recover ตัว database โดยใช้ database VM ตัวอื่น มาดูกันว่า NDB สามารถจัดการกับสถานการณ์ recovery เช่นนี้ได้อย่างไร

1.  ภายใน NDB เลือก **\> Data Protection > Time Machines** จาก drop-down จากนั้นคลิก **List** จากเมนูด้านบน
    
    ![](/images/09.7692af78.png)
    
2.  ต่อไป เลือกรายการ `User##`\-fiestadb\_TM เช่นเดียวกับที่คุณทำด้านบน
    
    ![](/images/01.57ac5cd0.png)
    
3.  คลิก **\> Actions > Create a Clone of the PostgreSQL Instance**
    
    ที่ด้านล่างของหน้าจอ ให้เลือก snapshot จาก dropdown จากนั้นคลิก **Next**
    
    ![](/images/05.506f8713.png)
    
    !!! note
        การ Recovering ไปยัง cloned copy จะช่วยให้คุณสามารถ recover ตัว tables หรือ rows ที่หายไป และ import พวกมันไปยัง affected database ได้
    
4.  ภายในส่วน _Database Server VM_ ให้กรอกข้อมูลในช่องดังต่อไปนี้ แล้วคลิก **Next**
    
    -   **Database Server VM** - Create New Server
    -   **Database Server VM Name** - `User##`\-postgres\_clone
    -   **Compute Profile** - CUSTOM\_EXTRA\_SMALL
    -   **Network Profile** - DEFAULT\_OOB\_POSTGRESQL\_NETWORK
    -   **SSH Public Key** - เลือก **Text** จากนั้นให้ copy/paste คีย์ด้านล่างลงใน text field
    
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoQRdEfm8ZJNGlYLQ2iw08eVk/Wyj0zl3M5KyqKmBTpUaS1uxj0K05HMHaUNP+AeJ63Qa2hI1RJHBJOnV7Dx28/yN7ymQpvO1jWejv/AT/yasC9ayiIT1rCrpHvEDXH9ee0NZ3Dtv91R+8kDEQaUfJLYa5X97+jPMVFC7fWK5PqZRzx+N0bh1izSf8PW0snk3t13DYovHFtlTpzVaYRec/XfgHF9j0032vQDK3svfQqCVzT02NXeEyksLbRfGJwl3UsA1ujQdPgalil0RyyWzCMIabVofz+Czq4zFDFjX+ZPQKZr94/h/6RMBRyWFY5CsUVvw8f+Rq6kW+VTYMvvkv
    ```
    
    ![](/images/06.22766092.png)
    
    !!! note    
        คุณสามารถทำ recovery ไปยัง authorized database vm ที่มีอยู่ได้ คุณไม่จำเป็นต้องสร้างขึ้นใหม่เสมอไป
    
5.  ภายในส่วน _Instance_ ให้กรอกข้อมูลในช่องดังต่อไปนี้ แล้วคลิก **Clone**
    
    -   **Name** - `User##`\-fiestadb\_clone
    -   **POSTGRES Password** - `nutanix/4u`
    -   **Database Parameter Profile** - DEFAULT\_POSTGRES\_PARAMS
    
    ![](/images/07.0c313407.png)
    
    !!! note
        NDB ยังมอบความสามารถในการรัน scripts หรือ commands ทั้งก่อนและหลังทำ database creation สิ่งเหล่านี้สามารถนำมาใช้เพื่อปรับแต่ง (customize) ตัว environment เพิ่มเติมตามความต้องการเฉพาะขององค์กรได้ เช่น การสร้าง sanitized clone จาก snapshot
    
6.  คลิกที่แถบสีน้ำเงินที่ด้านบนของหน้าจอเพื่อดูว่าการ restore ได้เริ่มต้นขึ้นแล้ว
    
    ![](/images/08.3d5a9409.png)
    
7.  จากหน้า operations page คุณสามารถดูความคืบหน้าของ clone creation และเวลาที่ใช้ไปในการ recover ตัว source database ใน task แรกของเรา
    
    ![](/images/11.54cc82d0.png)
    

และนี่คือภาพรวมของตัวเลือกบางส่วนที่คุณมีสำหรับทำ database recovery ด้วย NDB ในขณะที่ operations เหล่านั้นกำลังดำเนินการจนเสร็จสมบูรณ์ เราไปต่อที่ส่วน [Database Provisioning and Patching](/ndb-postgresql-provision/index.html) กันเลย