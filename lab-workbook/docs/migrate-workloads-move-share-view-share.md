# View Source Share

มาดูที่ source share ที่เรากำลังจะทำ migrating ไปยัง Nutanix Files กันก่อน นี่คือ common share ที่ lab users ทุกคนจะใช้ในการ migrate ดังนั้นมันจะเป็นการทำ 1-to-many migration เนื่องจาก users ทุกคนมี destination shares บน Files ของตัวเอง

1.  Login เข้าสู่ Prism Central
    
    -   **username** - `<PC login> adminuser##@ntnxlab.local.` หรือ `adminuser##`
    -   **password** - `<PC Password>` จาก Connection Details
2.  ไปที่ **Compute & Storage** > **VMs** ภายในส่วน Infrastructure ของ App Switcher
    
    ![](/images/vm-migration1.dcec209e.png)
    
3.  Windows VM จะถูกสร้างขึ้นสำหรับทุก user โดยใช้ชื่อว่า `Desktop##` ให้ค้นหา Desktop ที่ตรงกับคุณแล้วดึง IP address มา
    
    IMPORTANT
    
    ## จะตรงกับ user number ของคุณ โดยที่ # ตัวที่ 2 ของ user number ของคุณจะต้องตรงกับหมายเลขตัวที่ 2 ใน Desktop##
    
    example :
    
    User01, User11, User21, User31, User41 จะ map กับ Desktop01
    
    User02, User12, User22, User32, User42 จะ map กับ Desktop02
    
    และต่อไปเรื่อยๆ
    
    ![](/images/files-migration-source1.5d910e0d.png)
    
4.  ต่อไปให้ค้นหา `nextfiles` ซึ่งเป็น Nutanix Files instance ที่กำลัง running บน cluster และจะทำหน้าที่เป็น target ให้ Copy ตัว IP address ของมันไว้ เพราะจะจำเป็นต้องใช้ในภายหลัง
    
    ![](/images/files-migration-target1.6789c8d3.png)
    
5.  ก่อนที่จะทำขั้นตอนถัดไป คุณต้อง log into ตัว desktop ของคุณโดยใช้ Remote Desktop หากจำเป็น โปรดอ้างอิงคำแนะนำใน [the appendix](/migrate/appendix/remote_desktop.html) คุณสามารถค้นหา desktop ของคุณบน Prism Central ได้ด้วย naming convention `Desktop##` และใช้ IP Address เพื่อทำ remote desktop เข้าไป
    
    -   **username** - `adminuser##@ntnxlab.local`
    -   **password** - `<adminuser-password>`
6.  พิมพ์ `\\winfs-next\nextlab` ภายในฟิลด์ **Search** แล้วกด **Enter** หรือคลิกที่รายการ `\\winfs-next\nextlab` ที่ตามมาเพื่อเปิด source share
    
    ![](/images/files_source1next.1116e95d.png)
    
    ตรวจสอบ (Verify) ว่า source share มีไฟล์ขนาด 100MB อยู่ข้างใน นี่คือสิ่งที่เราจะทำการ migrate
    
    ![](/images/files_source2next.0a288822.png)
