# Add nodes to the chatflow

node คือการแสดงภาพของ component ที่ทำหน้าที่เฉพาะใน AI workflow ของเรา สำหรับ chatbot แบบง่ายนี้ เราจะใช้ 3 node

!!! info

    **Node Types**

    Flowise node คือ building block แบบ modular และปรับแต่งได้ใน visual editor ของ Flowise ที่แสดงถึงฟังก์ชันหรือ component แต่ละส่วน ใน chatbot นี้ คุณใช้ node ต่อไปนี้ที่อิงตาม Langchain framework:

    -   **ChatOpenAI Custom** - wrapper รอบๆ class `ChatOpenAI` ของ Langchain ที่ให้ความยืดหยุ่นโดยการปรับ parameter เพื่อเชื่อมต่อกับ custom endpoint
    -   **Conversation Chain** - เปิดใช้งานการสนทนาโต้ตอบโดยเก็บประวัติการสนทนาระหว่างผู้ใช้และ language model
    -   **Buffer Window Memory** - มอบ memory ให้กับ Conversation Chain เพื่อจัดเก็บและดึงการแลกเปลี่ยนในอดีต

    Flowise กำหนดให้ chatflow ต้องจบด้วย Chain (Langchain), Engine (LlamaIndex) หรือ Agent node ใน lab นี้เราใช้ Conversation Chain

1.  คลิกเครื่องหมาย **+** ที่มุมบนซ้ายเพื่อเพิ่ม node ใหม่
    
2.  ใต้ Langchain > Chat Models ค้นหา **ChatOpenAI Custom** แล้วลากไปวางบน canvas สามารถค้นหาด้วยคำว่า **ChatOpenAI**
    
    ![Flowise Nodes](images/flowise-chatopenai-custom.d41ecd91.png)![Drag and Drop](images/flowise-drag.580762bd.gif)
    
    !!! tip    
        Nutanix Enterprise AI endpoints รองรับ OpenAI-compatible API
    
3.  คลิกเครื่องหมาย **+** ที่มุมบนซ้ายเพื่อเพิ่ม node ใหม่อีกตัว
    
4.  ใต้ Langchain > Chains ค้นหา **Conversation Chain** แล้วลากไปวางบน canvas
    
5.  คลิกเครื่องหมาย **+** ที่มุมบนซ้ายเพื่อเพิ่ม node ตัวสุดท้าย
    
6.  ใต้ Langchain > Memory ค้นหา **Buffer Window Memory** แล้วลากไปวางบน canvas
    
7.  คลิกไอคอน **Save** เพื่อบันทึก chatflow ของคุณ

---

[← Back: Create a Blank Chatflow in Flowise](nai-application-chatbot-flowise.md) | [Home](nai-welcome.md) | [Next: Configure Nodes →](nai-application-chatbot-confnode.md)