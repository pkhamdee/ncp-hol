# Database Provisioning and Patching

การทำ database VM deployment แบบดั้งเดิมมีลักษณะคล้ายกับแผนภาพด้านล่าง กระบวนการนี้มักจะเริ่มต้นด้วย IT ticket สำหรับ database (จาก Dev, Test, QA, Analytics, ฯลฯ) จากนั้น ทีมจะต้อง deploy ตัว storage resources และ VM(s) ที่จำเป็น เมื่อ infrastructure พร้อมใช้งาน Database Administrator (DBA) จะต้องทำการ provision และ configure ตัว database software เมื่อทำการ provision เสร็จแล้ว จะต้องปรับใช้ best practices ซึ่งรวมถึง data protection และ backup policies ในท้ายที่สุด database จะสามารถส่งมอบให้กับ end user ได้ ซึ่งมีขั้นตอนการส่งมอบมากมายและอาจก่อให้เกิดปัญหาตามมาได้

![](/images/n00.4edf9d4c.png)

การทำ Provisioning และ protecting ให้กับ databases จะใช้เวลาและแรงลดลงอย่างมากเมื่อใช้ NDB ในขณะที่ดำเนินการตามขั้นตอนใน labs ให้พิจารณาว่าคุณจะดำเนินการ tasks เหล่านี้ใน existing environment ของคุณในปัจจุบันอย่างไรแล้วนำมาเปรียบเทียบกัน

## Provision PostgreSQL Database

ในฐานะ Database administrator (DBA) คุณจะต้องสร้าง database servers ใหม่ NDB ทำให้กระบวนการนั้นง่ายดายด้วย customized predefined profiles มาลองทำขั้นตอน provisioning สำหรับ PostgreSQL database กัน ไม่ต้องกังวลหากคุณไม่คุ้นเคยกับ PostgreSQL; NDB ทำให้กระบวนการนี้ง่ายขึ้น ขั้นตอนที่ทำในระหว่าง provisioning workflow นี้จะคล้ายคลึงกันสำหรับ databases ทั้งหมดที่รองรับโดย NDB

1.  ภายใน **NDB** ให้เลือก **\> Databases** จากเมนู จากนั้นเลือก **Sources** จากเมนูด้านบน
    
2.  คลิก **Provision > PostgreSQL > Instance**
    
3.  กรอกข้อมูลในช่องต่อไปนี้ในหน้า Database Server VM จากนั้นคลิก **Next**
    
    -   **Database Server VM** - Create New Server
    -   **Database Server VM Name** - `User##-mypsql`
    -   **Software Profile** - POSTGRES\_15.6\_ROCKY\_LINUX\_8\_OOB
    -   **Compute Profile** - CUSTOM\_EXTRA\_SMALL
    -   **Network Profile** - DEFAULT\_OOB\_POSTGRESQL\_NETWORK
    -   **SSH Public Key for Node Access** - เลือก **Text** จากนั้น copy/paste คีย์ด้านล่างลงใน text field
    
    ```
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoQRdEfm8ZJNGlYLQ2iw08eVk/Wyj0zl3M5KyqKmBTpUaS1uxj0K05HMHaUNP+AeJ63Qa2hI1RJHBJOnV7Dx28/yN7ymQpvO1jWejv/AT/yasC9ayiIT1rCrpHvEDXH9ee0NZ3Dtv91R+8kDEQaUfJLYa5X97+jPMVFC7fWK5PqZRzx+N0bh1izSf8PW0snk3t13DYovHFtlTpzVaYRec/XfgHF9j0032vQDK3svfQqCVzT02NXeEyksLbRfGJwl3UsA1ujQdPgalil0RyyWzCMIabVofz+Czq4zFDFjX+ZPQKZr94/h/6RMBRyWFY5CsUVvw8f+Rq6kW+VTYMvvkv
    ```
    
    ![](/images/n01a.39cf26d5.png)![](/images/n01b.0312fc1c.png)
    
    !!! note 
        SSH public key ด้านบนมีไว้เป็นตัวอย่างและถูกตั้งค่าให้เป็น authorized key สำหรับ operating system ที่ถูก provisioned โดย NDB ในสภาพแวดล้อมที่ไม่ใช่ lab คุณจะต้องสร้าง SSH private/public key pair ของคุณเอง และใช้ public key ในระหว่างขั้นตอนนี้
    
4.  กรอกข้อมูลในช่องต่อไปนี้ภายในหน้า _Instance_ จากนั้นคลิก **Next**
    
    -   **PostgreSQL Instance Name** - `User##-LabDB`
    -   **Database Parameter Profile** - DEFAULT\_POSTGRES\_PARAMS
    -   **Listener Port** - 5432 (default)
    -   **Size (GiB)** - 200 (default)
    -   **Name of Initial Database** - `User##initialdb`
    -   **Database user password** - `nutanix/4u`
    
    ![](/images/n04.3be8877f.png)
    
    !!! note    
        NDB ยังให้ความสามารถในการรัน scripts หรือ commands ทั้งก่อนและหลังการทำ database creation ซึ่งสามารถใช้เพื่อปรับแต่ง (customize) ตัว environment เพิ่มเติมตามความต้องการเฉพาะขององค์กรได้
    
5.  กรอกข้อมูลในช่องต่อไปนี้ภายในหน้า _Time Machine_ จากนั้นคลิก **Next**
    
    -   **Name** - `User##`\-LabDB\_TM (default)
    -   **SLA** - DEFAULT\_OOB\_GOLD\_SLA
    -   **Schedule** - (default)
    
    ![](/images/4f2.612bbe7a.png)
    
    !!! note    
        -   ส่วนของ Time Machine จะตั้งเวลาที่คุณต้องการทำ full snapshots ให้กับ database, วิธีการทำงานในหนึ่งวัน, และความถี่ที่คุณต้องการให้ log backups ทำงาน
        -   NDB time machines จะบันทึกและเก็บรักษา snapshots และ transaction logs ของ source databases ของคุณตามที่กำหนดไว้ใน Data Access Management policies (DAM policies) และ SLA schedules
    
6.  กรอกข้อมูลในช่องต่อไปนี้ภายในหน้า **Additional Configurations** จากนั้นคลิก **Provision**
    
    -   ภายใต้ **Opt in/out for repeated automated scheduling**
        -   ทำเครื่องหมายที่ **Operating System Patching**
        -   ทำเครื่องหมายที่ **Database Patching**
    -   **Maintenance Windows** - `user##-maintenance-policy`
    
    ![](/images/9.60703453.png)
    
    !!! note    
        -   ตัวเลือกในการเลือก maintenance policy ระหว่างการทำ provisioning เป็นอีกวิธีหนึ่งที่ NDB ช่วยให้ database VMs ของคุณได้รับการอัปเดตและปลอดภัย
        -   PostgreSQL และ MongoDB รองรับการทำ database encryption คุณสามารถศึกษาเพิ่มเติมได้ใน [NDB documentation](https://portal.nutanix.com/page/documents/list?type=software&filterKey=software&filterVal=Nutanix%20Database%20Service) บน Nutanix Portal
    
7.  เลือก **\> Operations** จาก drop-down menu เพื่อ monitor การทำ provisioning กระบวนการนี้จะใช้เวลา <20 นาที
    
    !!! note    
        operations ทั้งหมดภายใน NDB จะมี unique IDs และสามารถมองเห็นได้อย่างครบถ้วนสำหรับการทำ logging/auditing
    

ในขณะที่ database vm ของคุณกำลังอยู่ในช่วง provisioning เราไปต่อกันที่ส่วน [On Demand OS Patching](#on-demand-os-patching) กันเลย

## On Demand OS Patching

ในฐานะ DBA คุณอาจต้องรับผิดชอบในการทำ patching ตัว operating system เมื่อคุณจัดการ database servers จำนวนมาก คุณเพิ่งได้รับการแจ้งเตือนเกี่ยวกับ critical security flaw ใน database VMs ของคุณ ตัว patch ถูกพุชไปยัง repository แล้ว ดังนั้นคุณจะต้องอัปเดต affected VMs database อย่างรวดเร็ว มาเริ่มกันเลย

1.  เลือก **\> Database Server VMs** จากนั้นเลือก **list** จากเมนูด้านบน
    
    ![](/images/20.4255e03b.png)
    
2.  คลิกที่ `User##`\-postgres ของคุณจากรายการ VMs ซึ่งควรจะนำคุณไปยังหน้า database servers details ตามที่แสดงด้านล่าง
    
    ![](/images/21.d8923698.png)
    
3.  เลือก **\>Actions > Patch OS Now**
    
4.  บนหน้าจอ **Patch OS Now** ให้ดำเนินการดังต่อไปนี้แล้วคลิก **Update**
    
    -   ขยายส่วน **Custom Commands > Operating System Patching Command** : ป้อน `sudo dnf update -y --security`
    -   ภายใต้ **Reboot Needed** : เลือก Yes
    
    ![](/images/22.e7d1968a.png)
    
    !!! note    
        -   เราเลือกที่จะ reboot yes ในกรณีนี้ แต่คุณจะได้รับตัวเลือกให้ reboot ภายหลังได้หากจำเป็น
        -   เนื่องจากเป็น security flaw เราจึงเลือกตัวเลือก --security สำหรับ RHEL based VMs
        -   คุณสามารถใช้ yum แทนใน RHEL 9 based และ distribution รุ่นก่อนหน้าได้
        -   สำหรับ Ubuntu (Debian) based distributions คุณสามารถตั้งค่า unattended-upgrade package เพื่อทำ security updates เท่านั้น สำหรับข้อมูลเพิ่มเติม โปรดดู distribution's documentation ของคุณ ซึ่งสามารถรันได้โดยใส่ sudo apt-get `package_name` ลงใน custom command
    
5.  คลิกที่แถบสีเขียวเพื่อตรวจสอบ progress ของ patching operation
    
    ![](/images/23.f98533e1.png)
    

นี่คือความรวดเร็วที่คุณสามารถทำ patch ให้กับ day-zero vulnerability บน database VM operating system ด้วย NDB คุณจะต้องทำซ้ำขั้นตอนนี้กับ affected VMs อื่นๆ ใน environment ของคุณ NDB จะใช้ patch repository ที่ตั้งค่าไว้บน database VM เพื่อดึง patches มา อย่างไรก็ตาม สิ่งนี้จะทำการ patch ตัว database operating system เท่านั้น

NDB สามารถทำ patch ให้กับ database servers ของคุณเป็นจำนวนมาก (in bulk) โดยใช้ Maintenance Window ที่เราได้ตั้งค่าไว้เมื่อเริ่ม lab ในขณะที่ database OS ของคุณกำลังทำ patching เรามาดูวิธีตั้งค่า scheduled bulk patching กัน

## Scheduled Database Patching

1.  เลือก **\>Database Server VMs** จากนั้นคลิก **List** บนเมนูด้านบน
    
    ![](/images/24.be216d06.png)
    
2.  เลือก `user##-postgres` VM ของคุณจากรายการ
    
    ![](/images/25.99dd16e3.png)
    
3.  เลือก Actions Dropdown จากนั้นเลือก Maintenance
    
    ![](/images/26.e4d8b16b.png)
    
4.  เลือกตัวเลือกต่อไปนี้:
    
    -   **Operating System Patching** - Check
    -   **Database Patching** - Check
    -   **Maintenance Window** - เลือก `user##-maintenance-policy`
    
    ![](/images/27.d1bce96a.png)
    
    คุณสามารถเลือกที่จะ schedule ตัว OS Patching หรือ Database patching อย่างใดอย่างหนึ่งหรือทั้งสองอย่างก็ได้ นอกจากนี้ คุณยังสามารถเลือก multiple VMs พร้อมกันเพื่อเชื่อมโยง (associate) กับ maintenance window policy ได้ เมื่อคุณเลือกเสร็จแล้ว ให้คลิก **Associate**
    
    !!! note    
        -   ปัจจุบัน PostgreSQL, MongoDB, และ Oracle รองรับการทำ bulk patching ด้วย Maintenance Window Policies สำหรับ database software updates และ operating systems
        -   ปัจจุบัน Microsoft SQL server รองรับเฉพาะ database software updates โดยใช้ฟีเจอร์ Maintenance Window Policy เท่านั้น
    

ตอนนี้เราได้ตั้งค่า patching สำหรับ database VM ของเราเรียบร้อยแล้ว เราไปต่อกันที่ส่วนของ [clone creation and management](/ndb-postgresql-create-manage/index.html) กันเลย


---

[← Back: Database Recovery](ndb-postgresql-dr.md) | [Home](ndb-getting-started.md) | [Next: Create and Manage Clones →](ndb-postgresql-create-manage.md)