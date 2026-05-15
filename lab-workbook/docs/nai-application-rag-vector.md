# Configure Embeddings and Vector Store

!!! tip

    input ที่คุณต้องการสามารถดูได้จากหน้า Connection Details หรือจาก instructor ครั้งนี้เราจะใช้โมเดล **Embedding**

    -   BaseURL: `Shared NAI Endpoint URL`
    -   Model Name: `Shared NAI Embedding Model Name`
    -   OpenAI API key: `Shared NAI API Key`

## View Chunks

1.  Document Loader ของคุณควรแสดง 17 chunks ในขณะนี้ หากต้องการดู chunk คลิกที่ชื่อ Document Loader
    
    ![Document Chunks](images/document-chunks.afdb2f84.png)
    
2.  คลิกลูกศร **Back** สีม่วง
    

## Configure Embeddings

1.  ใต้คอลัมน์ Actions ในตาราง **documents** คลิก **Options** > **Upsert Chunks**
    
    ![Upsert Chunks](images/upsert-chunks.f8dc7631.png)
    
2.  คลิก **Select Embeddings**
    
    ![Select Embedding](images/configure-embeddings.20eae8e4.png)
    
3.  ค้นหา **OpenAI Embeddings Custom**
    
    ![OpenAI Embeddings Custom](images/openai-embeddings-custom.764f9f19.png)
    
4.  ใต้ **Connect Credential** เลือก Shared Key credential ที่กำหนดค่าไว้ในส่วน [Configure Shared Endpoint](nai-application-chatbot-shared.md)
    
5.  ใต้ **BasePath** ป้อน `Shared NAI Endpoint URL`
    
6.  ใต้ **Model Name** ป้อน `Shared NAI Embedding Model Name`
    

หน้าจอของคุณควรมีลักษณะคล้ายกับด้านล่าง

![Configured Embeddings](images/after-embeddings.11db74f0.png)

!!! warning

    ตรวจสอบให้แน่ใจว่าคุณใช้โมเดล **Embedding** ที่ใช้ร่วมกันตามที่อ้างอิงไว้ด้านบน มิฉะนั้น คุณจะพบข้อผิดพลาด 504 เมื่อทำการ upsert

## [#](#configure-vector-store) Configure Vector Store

1.  คลิก **Select Vector Store**
    
2.  เลือก **Milvus**
    
3.  ใต้ Connect Credential คลิก **Create New**
    
4.  ป้อน root สำหรับชื่อ credential และ Milvus user และปล่อย password ว่างไว้ จากนั้นคลิก **Add**
    
    ![Milvus Auth](images/milvus-auth.564c6d09.png)
    
    !!! tip
    
        เราใช้ root user เพื่อวัตถุประสงค์ในการสาธิต ในการใช้งานจริง ไม่แนะนำให้ใช้ root user
    
5.  ใต้ **Milvus Server URL** ป้อน URL ไปยัง Milvus Database ที่ตรงกับ user ของคุณ
    
    !!! warning
    
        อย่าใช้ URL ที่มี **attu** ในชื่อ เพราะนั่นสำหรับ UI ไม่ใช่ database
        
        อย่าใส่ trailing slash ที่ท้าย URL
    
6.  ใต้ **Milvus Collection Name** ป้อน **docsuser`##`** โดยที่ `##` ตรงกับหมายเลข user ของคุณ
    
7.  ที่มุมบนขวา คลิก **Save Config** แล้วคลิก **Upsert**
    
    ![Save and Upsert](images/save-and-upsert.948d89c5.png)
    
8.  เมื่อ upsert สำเร็จ คุณควรเห็นหน้าจอดังต่อไปนี้ คลิก **Close**
    
    ![Successful Upsert](images/successful-upsert.12cb21bf.png)
    

ตอนนี้คุณพร้อมเชื่อมต่อ chatflow กับ vector database แล้ว

## View Documents in Milvus (optional)

1.  เปิดแท็บใหม่ในเบราว์เซอร์และไปยัง Milvus Attu UI ที่ตรงกับ database ของคุณ
    
2.  คลิก **Connect** เพื่อเข้าสู่ระบบ
    
    ![Milvus Login](images/milvus-login.d7400e67.png)
    
3.  คลิก database **default**
    
4.  คุณควรเห็น collection ที่สร้างโดยกระบวนการ upsert
    
5.  คลิกป้าย **unloaded** แล้วคลิก **Load**
    
    ![Load Collection](images/milvus-unloaded.50efd291.png)
    
6.  เมื่อสถานะเปลี่ยนเป็น **loaded** ให้คลิกชื่อ collection แล้วคลิกแท็บ **Data**
    
7.  เลื่อนไปทางขวาเพื่อดูข้อความและ vector ที่สอดคล้องกัน
    
    ![Vectors](images/milvus-vectors.ac30bc1e.png)

---

[← Back: Configure Document Store](nai-application-rag-confstore.md) | [Home](nai-welcome.md) | [Next: Configure Chatflow →](nai-application-rag-chatflow.md)