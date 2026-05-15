# Configure Chatflow

## Duplicate Chatflow

เมื่อกำหนดค่า Document Store แล้ว คุณสามารถนำมาใช้ใน chatflow ได้

มาเริ่มด้วยการ duplicate chatflow ที่มีอยู่

1.  จาก Flowise user interface คลิก **Chatflows** ในเมนู
    
2.  เปลี่ยนเป็น table view โดยคลิกไอคอน List View
    
    ![Table View](images/chatflow-table-view.df4a0580.png)
    
3.  ใต้ **Options** คลิก **Duplicate**
    
    ![Duplicate Chatflow](images/duplicate-chatflow.ffeaaa25.png)
    
    canvas chatflow ใหม่จะเปิดขึ้นพร้อม chatflow ที่ duplicate มา
    
4.  คลิก **Save** และตั้งชื่อให้กับ chatflow ที่ duplicate มา
    
    ![Save Icon](images/save-icon0.5bed6b46.png)
    

## Configure Chatflow for RAG

!!! info

    **Node Types**

    เราได้แนะนำ Flowise node ในส่วน [Add Nodes to Chatflow](nai-application-chatbot-connnode.md) สำหรับ chatbot ของคุณ คุณใช้ node ประเภทต่อไปนี้:

    -   **ChatOpenAI Custom** - wrapper รอบๆ class `ChatOpenAI` ของ Langchain ที่ให้ความยืดหยุ่นโดยการปรับ parameter เพื่อเชื่อมต่อกับ custom endpoint
    -   **Conversation Chain** - เปิดใช้งานการสนทนาโต้ตอบโดยเก็บประวัติการสนทนาระหว่างผู้ใช้และ language model
    -   **Buffer Window Memory** - มอบ memory ให้กับ Conversation Chain เพื่อจัดเก็บและดึงการแลกเปลี่ยนในอดีต

    สำหรับการ implement Retrieval Augmented Generation (RAG) เราจะใช้ node ทั้งหมด 4 ตัว:

    -   **ChatOpenAI Custom** - ไม่เปลี่ยนแปลง
    -   **Buffer Window Memory** - ไม่เปลี่ยนแปลง
    -   **Conversational Retrieval QA Chain** - แทนที่ node **Conversation Chain** node นี้มีฟังก์ชันคล้ายกัน แต่รวม input **Vector Store Retriever** ด้วย
    -   **Document Store (Vector)** - การเชื่อมต่อกับ Document Store ที่กำหนดค่าไว้ในส่วนก่อนหน้า

1.  ใต้ node **ChatOpenAI Custom** ตรวจสอบให้แน่ใจว่าคุณใช้รายละเอียด shared inference endpoint สำหรับ **Connect Credential**, **Model Name** และ **Additional Parameters** > **BasePath**
    
    ![ChatOpenAI Custom Node](images/chatopenai-shared.7d2a4ce7.png)![ChatOpenAI Custom Node BasePath](images/chatopenai-basepath.6af5d6cb.png)
    
2.  วางเมาส์เหนือ Conversation Chain node แล้วคลิกไอคอนถังขยะเพื่อลบ node
    
    ![Delete Conversation Chain](images/delete-conversation-chain.a1375f23.png)
    
3.  ทางด้านซ้ายมือ คลิกเครื่องหมาย **+** เพื่อเพิ่ม node ใหม่
    
4.  ใต้ **Langchain > Chains** เลือก node **Conversational Retrieval QA Chain** และลากไปวางบน canvas
    
    ![Conversational Retrieval QA Chain](images/add-conversational-retrieval-chain-node.dd8ed775.png)
    
5.  ทางด้านซ้ายมือ คลิกเครื่องหมาย **+** เพื่อเพิ่ม node ใหม่อีกตัว
    
6.  ใต้ **Langchain > Vector Stores** เลือก node **Document Store (Vector)** และลากไปวางบน canvas
    
    ![Document Store (Vector)](images/select-document-store-vector-node.6da57bef.png)
    
    ณ จุดนี้ canvas ของคุณควรมีลักษณะคล้ายกับด้านล่าง:
    
    ![RAG Chatflow Canvas](images/rag-flow-disconnected.5fc60b24.png)
    
7.  ใน Document Store node จาก drop down **Select Store** เลือก Document Store **documents** ที่สร้างในส่วนก่อนหน้า
    
8.  ลาก output connector **Retriever** ไปยัง input **Vector Store Retriever** บน Conversational Retrieval QA Chain node
    
    ![Connecting Document Store to QA Chain](images/rag-connect1.aa0099f9.png)
    
9.  ใน ChatOpenAI Custom node ลาก output connector **ChatOpenAI-Custom** ไปยัง input **Chat Model** บน Conversational Retrieval QA Chain node
    
10.  ใน Buffer Window Memory node ลาก output connector **BufferWindowMemory** ไปยัง input **Memory** บน Conversational Retrieval QA Chain node
    
11.  ใน Conversational Retrieval QA Chain node เปิดใช้งาน **Return Source Documents**
    
    canvas ของคุณควรมีลักษณะคล้ายกับด้านล่างนี้:
    
    ![Completed RAG Chatflow](images/rag-connect-final.367a7b32.png)
    
12.  คลิกไอคอน **Save** ที่มุมบนขวาเพื่อบันทึก chatflow ใหม่ของคุณ

---

[← Back: Configure Embeddings and Vector Store](nai-application-rag-vector.md) | [Home](nai-welcome.md) | [Next: Test the Chatbot →](nai-application-rag-test.md)