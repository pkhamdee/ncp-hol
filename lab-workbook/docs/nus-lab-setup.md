# Lab Setup

1.  เปิด Chrome, ป้อน IP address ของ Prism Element ของคุณ และเข้าสู่ระบบด้วย credentials ที่ระบุไว้ให้
    
2.  ในเมนู drop-down ให้เลือก **VM** แล้วคลิกที่ **Table**
    
3.  ค้นหาทั้ง **`User##`\-WinTools** และ **`User##`\-LinuxTools** VMs ของคุณ จดบันทึก IP addresses ของพวกมันไว้ โดยใช้คอลัมน์ **IP Addresses**
    
    ![](/images/ip_addresses.63dcc6c9.png)

4.  เชื่อมต่อไปยัง **`User##`\-WinTools** VM ของคุณผ่าน Remote Desktop โดยใช้ credentials ต่อไปนี้:
    
    -   Username **administrator@ntnxlab.local**
    -   Password: **cluster password**

สามารถดูคำแนะนำโดยละเอียดเพิ่มเติมได้ [ที่นี่](/unified-storage/appendix/remote_desktop.html)

5.  ภายใน **`User##`\-WinTools** VM ของคุณ ให้ดาวน์โหลดไฟล์ sample data ต่อไปนี้โดยคัดลอกและวาง URL นี้ลงใน Chrome

    ```
    http://10.42.194.11/hol/unified-storage/SampleData_Small.zip
    ```
