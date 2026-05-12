# Verify the Policy

ตอนนี้ production policy ของเราถูกนำมาใช้งานแล้ว มาตรวจสอบกันว่า traffic Flows ทั้งหมดของเราเป็นไปตามที่คาดไว้หรือไม่ 😉

## Access the Production App

1.  ใน Prism Central ไปที่ **Infrastructure** > **Compute** > **VMs**
    
2.  ค้นหา VM IP address ของ **user`##`\-prod-web** ของคุณ
    
    -   จดบันทึกไว้ คุณจะต้องใช้สำหรับการทดสอบ

    ![PROD-ToDO-App](images/verify-prod-01.e35e3dde.png)

### Verify Ping Access to the Application

1.  จากมุมมอง Prism Central VM ให้เลือก VM **user`##`\-enduser** ของคุณ
    
2.  เปิด console session ไปยัง VM โดยการคลิกขวาที่ VM หรือเลือก VM แล้วไปที่ **Launch Console**
    
    ![PROD-ToDO-App](images/verify-prod-02.894c657e.png)

3.  หากมีการแจ้งเตือน (prompted) ให้ login ให้ใช้ username nutanix และ cluster password ที่ให้ไว้เมื่อเริ่ม lab
    
4.  จากภายใน **user`##`\-enduser** VM console session ให้เปิด terminal session โดยใช้ไอคอน terminal ที่ด้านซ้ายล่าง
    
    ![PROD-ToDO-App](images/create-dev-26.2c23295c.png)

5.  ใน terminal session ให้ ping ไปยัง IP address ของ **user`##`\-prod-web** VM ที่คุณจดบันทึกไว้ก่อนหน้านี้

    ```
    ping 'user##-prod-web-ip'
    ```

    ![PROD-ToDO-App](images/verify-prod-04.5c76f3e5.png)

    การ pings ควรจะสำเร็จเนื่องจาก policy ของเราอยู่ใน monitor mode ปล่อยให้ pings เหล่านี้รันต่อไป

    เราจะ enforce ตัว policy นี้ในภายหลัง เนื่องจาก pings ไม่ควรถูก allowed ใน production application ของเราตาม requirements ก่อนหน้านี้

### Verify Web Access to the Application

access เดียวที่เราควร allow ให้กับ application คือผ่านทาง web ดังนั้นมาตรวจสอบให้แน่ใจว่ามันทำงานได้

1.  ภายใน **user`##`\-enduser** VM console เดียวกัน ให้เปิดเบราว์เซอร์ Firefox

    ![PROD-ToDO-App](images/verify-prod-05.2cfe82f1.png)

2.  ในแถบที่อยู่ของเบราว์เซอร์ ให้พิมพ์ address ของ **user`##`\-prod-web** VM ที่คุณบันทึกไว้ก่อนหน้านี้
    
    -   รูปแบบควรจะเป็น `http://IP.address.of.prod-web.VM`

    ![PROD-ToDO-App](images/verify-prod-06.a2f3e643.png)

4.  กด Enter

    คุณควรจะเห็น Prod ToDo App ของคุณรันอยู่ สิ่งนี้แสดงให้เห็นว่าคุณสามารถเข้าถึง application ได้ และ application สามารถเข้าถึง database ได้

    ![PROD-ToDO-App](images/verify-prod-07.4dbc0d62.png)

5.  ย่อ (Minimize) หรือย้ายหน้าต่าง VM console เพื่อให้เราสามารถกลับไปดู security policy ได้

## Check the Security Policy

ตอนนี้มาดูที่ **User`##`\-ProdSecurityPolicy**

ใน Prism Central ไปที่ **Infrastructure** > **Network & Security** > **Security Policies** > **Policy Type: Application** และเลือก **User`##`\-ProdSecurityPolicy**

![PROD-ToDO-App](images/verify-prod-08.6440f452.png)

ด้วย security policy ของเราใน Monitor mode เราจะเห็น ping traffic จาก **user`##`\-enduser** VM แสดงขึ้นมาเป็น discovered traffic ในสีเหลือง

![PROD-ToDO-App](images/verify-prod-09.0ee6815a.png)

### Enforce the Security Policy

มาเปลี่ยน mode ของ policy นี้จาก Monitor เป็น Enforce และดูว่าเกิดอะไรขึ้น

1.  ขยาย Apply dropdown ที่มุมขวาบนของหน้า policy details
    
2.  เลือก **Apply (Enforce)**
    
    ![PROD-ToDO-App](images/verify-prod-10.d78d14b1.png)

3.  ใน confirmation box ให้พิมพ์ **ENFORCE** และเลือก **Confirm**

    ![PROD-ToDO-App](images/verify-prod-11.7c8045b7.png)

## Check the Application Again

1.  นำทางกลับไปยัง **user`##`\-enduser** VM console session
    
2.  เปิด terminal session ใน VM ขึ้นมา
    
    -   คุณยังสามารถ ping ไปยัง **user`##`\-prod-web** VM ได้สำเร็จหรือไม่?

    ![PROD-ToDO-App](images/verify-prod-12.a85f680b.png)

    traffic ประเภทนี้จาก **user`##`\-enduser** VM ของเราไปยัง **user`##`\-prod-web** VM ไม่ได้รับอนุญาต (not permitted) ใน policy ของเรา เมื่อ security policy ถูก Enforced แล้ว traffic นี้กำลังถูก dropped

    ![PROD-ToDO-App](images/verify-prod-12.a85f680b.png)

3.  สุดท้าย กลับไปที่ console ของเราสำหรับ **user`##`\-enduser** VM
    
4.  ใช้ Firefox ภายใน VM's console เพื่อเปิด IP address ของ **user`##`\-prod-web** VM ของคุณ
    
    -   คุณควรจะเห็นหน้า web สำหรับ Prod ToDo application ของคุณ

    ![PROD-ToDO-App](images/verify-prod-06.a2f3e643.png)

5.  เพิ่ม Item ลงใน ToDo app

    ![PROD-ToDO-App](images/verify-prod-15.1522a7a3.png)

    action นี้จะสร้าง item ใหม่ใน database ของเรา

    ดังนั้นเป็นการทดสอบ connectivity จาก:

    -   **AppTier:User`##`Web**
    -   **AppType:User`##`ToDo**
    -   **Environment:User`##`Prod**

    ไปยัง:

    -   **AppTier:User`##`DB**
    -   **AppType:User`##`ToDo**
    -   **Environment:User`##`Prod**

การทดสอบนี้แสดงให้เห็นว่า ports ที่จำเป็นเพื่อให้ application ของเราสามารถทำงานได้นั้นถูกเปิดไว้ และสิ่งอื่นๆ ทั้งหมดเช่น ping จะถูก protected โดย security policy

![PROD-ToDO-App](images/verify-prod-16.dfe7b571.png)

มันไม่ได้ยากเกินไปใช่ไหม?

นี่คือมุมมองอีกแบบของสิ่งที่เราเพิ่งสร้างขึ้น

![PROD-ToDO-App](images/create-dev-51b.2b84559c.png)

### From the VM Perspective

เรายังสามารถตรวจสอบ associated policies และ categories จากมุมมองของหนึ่งใน web VMs ของเราได้ ลองตรวจสอบที่ dev แต่คุณก็สามารถตรวจสอบที่ prod ได้เช่นกัน

1.  ไปที่ **Infrastructure** > **Compute** > **VMs**
    
2.  คลิกที่ชื่อ dev web VM ของคุณ **user`##`\-dev-web**
    
3.  เลือก Categories
    
    ![Category Mapping](images/verify-prod-17.5e99207b.png)

4.  สังเกตว่าเราสามารถมองเห็น associated categories และ security policies ที่ถูก mapped เข้ากับ VM ผ่าน categories เหล่านั้นได้

## Takeaways

Flow Network Security Next-Gen เป็นเครื่องมือที่ทรงพลังและยืดหยุ่นสำหรับการ securing ตัว applications ของคุณ คุณสามารถผสมผสาน (Combine) ตัว Flow Network Security policies หลายประเภทเพื่อทำ protection ให้กับ applications ของคุณ ใช้ฟีเจอร์ต่างๆ เช่น การ saving ตัว policy โดยไม่ต้อง applying, การ cloning ตัว policy เพื่อเริ่มต้นด้วย baseline ที่ดี, และ monitor mode เพื่อให้แน่ใจว่า policy จะบรรลุเป้าหมายที่ต้องการโดยไม่ทำให้สิ่งใดพังทลาย

ขอแสดงความยินดี คุณทำได้แล้ว! 🎉
