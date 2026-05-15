# Obtain Object Store URL and Credentials

เพื่อดู Object Store URL และ credentials ที่ใช้เฉพาะสำหรับ environment ของคุณ คุณจะต้องเข้าสู่ระบบ Prism Central

1.  จากเบราว์เซอร์ Chrome ป้อน IP address ของ Prism Central ในแท็บเบราว์เซอร์ใหม่

!!! tip
    IP ของ Prism Central สามารถดูได้จากหน้า Connection Details หรือสอบถาม instructor

![](images/pc-login.67582df8.png)

1.  เข้าสู่ระบบ Prism Central ตรวจสอบให้แน่ใจว่าหน้า Login แสดง **Login with your Company ID**
    
    -   Username: `<PC Username>` (เช่น adminuser##@ntnxlab.local)
    -   Password: `<PC Password>`

2.  เมื่อเข้าสู่ระบบแล้ว คลิก drop down และไปที่ Self Service
    
    ![](images/pc-apps-menu.4b00d211.png)
    
3.  จากเมนูทางซ้ายมือ คลิก **Applications**
    
    ![](images/ncm-applications.3a355033.png)
    
4.  คลิก **Manage** ใต้แอป `nai`
    
    ![](images/manage-app.5da58fc5.png)
    
5.  หน้าจอถัดไปจะแสดง app summary ที่มุมบนซ้ายมี widget ที่แสดงข้อมูลเกี่ยวกับแอป รวมถึงลิงก์ไปยัง NAI dashboard และข้อมูล Objects คลิกลูกศรลงเล็กๆ เพื่อขยาย
    
    ![](images/view-app-info.57392f0e.png)
    
6.  คลิกลิงก์ Objects Browser เพื่อเปิด Objects browser ในแท็บใหม่
    
7.  Copy และ paste Access และ Secret Keys ลงใน text editor เพื่อใช้อ้างอิงในภายหลัง

---

[← Back: Configure RAG Overview](nai-application-rag.md) | [Home](nai-welcome.md) | [Next: Upload Document to Object Store →](nai-application-rag-upload.md)