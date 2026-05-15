# Configure Document Store

## Create document store

1.  จาก chatflow ของคุณ ตรวจสอบให้แน่ใจว่า chatflow ถูกบันทึกแล้ว จากนั้นคลิกลูกศรย้อนกลับ
    
    ![Chatflow Back Button](images/chatflow-back-button.7d402437.png)
    
2.  คลิก Document Stores ในเมนูทางซ้าย หากเมนูถูกซ่อนอยู่ ให้คลิกไอคอน hamburger สีม่วงข้างๆ โลโก้ FlowiseAI
    
    ![Navigate to Document Stores](images/navigate-to-ds.8b3ede50.png)
    
3.  คลิก **+Add New** ที่มุมบนขวา
    
    ![Add New Document Store](images/add-new-ds.ce91ec5b.png)
    
4.  ตั้งชื่อ Document Store เช่น `documents` แล้วคลิก **Add**
    
    ![Add Document Store](images/add-new-ds-dialog.676c0f8a.png)
    

## Add a document loader

1.  คลิกเข้าไปใน Document Store และคลิก **\+ Add Document Loader**
    
    ![Add Document Loader](images/add-document-loader.642da441.png)
    
2.  ค้นหา **S3** และคลิก **S3 Directory** ซึ่งจะช่วยให้เรากำหนดค่า Object Storage bucket ที่ต้องการใช้
    
    ![S3 Directory](images/s3-directory.86d4a89e.png)
    

## Configure document loader

1.  คลิก drop-down สำหรับ **Credential** และคลิก **Create New**
    
2.  ป้อนชื่อสำหรับ credential ของคุณ (เช่น `objects-creds`)
    
3.  ใต้ AWS Access Key และ AWS Secret Key ป้อน Access และ Secret Key ของ Nutanix Objects bucket ที่ดูได้จากส่วน [Obtain Object Store URL and Credentials](nai-application-rag-store.md) แล้วคลิก **Add**
    
    ![S3 Credentials](images/s3creds.8d6c1c03.png)
    
4.  ใต้ **Bucket** ป้อนชื่อ bucket ของคุณ เช่น `adminuser##`
    
5.  ปล่อย region ไว้ที่ค่า default
    
6.  ใต้ **Server URL** ป้อน IP ของ object store เช่น `http:/#.#.#.8`
    
    !!! warning    
        ใน lab นี้เราใช้ self-signed certificate ดังนั้นตรวจสอบให้แน่ใจว่าใช้ `http` ไม่ใช่ `https`
    
7.  ใต้ **Select Text Splitter** เลือก **Recursive Character Text Splitter** จาก drop-down
    
    ![Text Splitter](images/doc-store-text-splitter.604f8d7c.gif)
    
    ณ จุดนี้ หน้าจอของคุณควรมีลักษณะคล้ายกันนี้:
    
    ![S3 Directory Config](images/s3-configuration.10dd1f7b.png)
    
8.  ทางด้านขวามือ คลิก **Preview Chunks**
    
9.  preview chunks ควรปรากฏทางด้านขวามือ หากไม่ปรากฏ ให้ตรวจสอบ input สำหรับการเชื่อมต่อ Objects ของคุณ
    
    ![Preview Chunks](images/s3-preview-chunks2.343742c4.png)
    
10.  ที่มุมบนขวา คลิก **Process**
    
    ![Process Button](images/s3-process-button.c3fd4fcd.png)
    
11.  document store จะแสดงสถานะ "stale" พร้อมไอคอนวงกลมสีเหลือง คลิกปุ่ม refresh สีน้ำเงิน
    
    ![Refresh](images/s3-doc-store-refresh.70f6c46c.png)
    
12.  สถานะ "stale" ควรเปลี่ยนเป็น "sync" และไอคอนควรเปลี่ยนเป็นสีเขียว หากไม่เปลี่ยน ให้ตรวจสอบการตั้งค่า Document Loader อีกครั้ง
    
    ![Successful Sync](images/s3-synced.a7258f62.png)

---

[← Back: Upload Document to Object Store](nai-application-rag-upload.md) | [Home](nai-welcome.md) | [Next: Configure Embeddings and Vector Store →](nai-application-rag-vector.md)