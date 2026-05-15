# Configure Shared Endpoint

ในส่วนถัดไป เราจะใช้ shared inference endpoint ที่รองรับด้วย L40S GPU มาเตรียม chatbot ที่มีอยู่โดยอัปเดตรายละเอียด configuration ที่จำเป็น

!!! tip

    input ที่คุณต้องการสามารถดูได้จากหน้า Connection Details หรือจาก instructor

    -   BaseURL: `Shared NAI Endpoint URL`
    -   Model Name: `Shared NAI Text Generation Model Name`
    -   OpenAI API key: `Shared NAI API Key`

1.  บน **ChatOpenAI Custom** node คลิก drop down ใต้ **Connect Credential**
    
2.  คลิก **Create New**
    
3.  ตั้งชื่อ key และ copy shared API key เข้ามา แล้วคลิก **Add**
    
    ![API Key Configuration](images/api-key-config-shared.a20b6022.png)
    
4.  ใต้ **Model Name** พิมพ์ `Shared NAI Text Generation Model Name`
    
    ![OpenAI Node Config Shared](images/openai-node-shared.a571f03b.png)
    
5.  คลิก **Additional Parameters**
    
6.  เพิ่ม Max Token size เป็น 4096 และเปลี่ยน BaseURL เป็น `Shared NAI Endpoint URL`
    
    ![Additional Parameters](images/additional-params-shared.9311dac2.png)
    
7.  คลิกไอคอน **Save** เพื่อบันทึก chatflow
    
    ![Save Icon](images/save-icon.27cb1f8c.png)
    
8.  คลิกไอคอน chat สีม่วงด้านล่างไอคอนเฟือง
    
    ![Chat Icon](images/chat-icon.1529c512.png)
    
9.  ลองใช้ chatbot อีกครั้งโดยถามคำถามเดิมหรือคำถามอื่น และสังเกตว่าประสิทธิภาพดีขึ้น 🏎️

---

[← Back: Test Chatflow](nai-application-chatbot-test.md) | [Home](nai-welcome.md) | [Next: Configure RAG Overview →](nai-application-rag.md)