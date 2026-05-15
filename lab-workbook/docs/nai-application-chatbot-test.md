# Test Chatflow

1.  คลิกไอคอน **Save** เพื่อบันทึก chatflow ของคุณ
    
    ![Save Icon](images/save-icon0.5bed6b46.png)
    
2.  คลิกไอคอน chat สีม่วงด้านล่างไอคอนเฟือง
    
    ![Chat Icon](images/chat-icon.1529c512.png)
    
3.  ถามคำถาม เช่น `What is there to do in Chicago, IL?` แล้วรอคำตอบ เนื่องจาก inference engine รันบน CPU คำตอบอาจใช้เวลา 1-2 นาที ขึ้นอยู่กับว่า CPU รองรับ AMX หรือไม่
    

!!! tip
    ขณะที่คำตอบกำลัง streaming คุณสามารถดูการอัปเดต log จาก endpoint logs บน endpoint dashboard ใน Nutanix Enterprise AI ตามที่อธิบายไว้ใน [View the Endpoint Details](nai-fundamentals-endpoint-view.md)

หลังจากผ่านไปสองสามนาที ข้อมูล request จะปรากฏใน endpoint dashboard

![Endpoint Dashboard](images/endpoint-dashboard-after-request.caed0ecd.png)

คุณยังสามารถดูรายละเอียด metrics ได้โดยคลิก **Metrics > Usage** หรือ **Metrics > Performance**

---

[← Back: Connect Nodes](nai-application-chatbot-connnode.md) | [Home](nai-welcome.md) | [Next: Configure Shared Endpoint →](nai-application-chatbot-shared.md)