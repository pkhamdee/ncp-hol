# Create a RAG pipeline to chat with your documents

## RAG-enabled Chatbot Flow

กระบวนการทำงานของ chatbot ที่ใช้ RAG มีลักษณะดังแผนภาพด้านล่าง

![Chatbot Diagram](images/chatbot-with-rag-diagram.f81e33a9.png)

1.  **Ask Question**
    
    -   ผู้ใช้ถามคำถามกับ chatbot

2.  **Create Embedding of Query**
    
    -   แทนที่จะไปยัง inference API โดยตรง Flowise จะสร้าง embedding ของ query ก่อนโดยใช้โมเดล **embedding** ที่ host อยู่บน Nutanix Enterprise AI

3.  **Search/Retrieval of Similar Content**
    
    -   ด้วย embedding นั้น Flowise จะค้นหา embedding ที่คล้ายกันใน vector database ที่ถูกเติมด้วย embedding ของเอกสารต้นทาง

4.  **Send Prompt to Inference API**
    
    -   Flowise เพิ่ม context ที่พบเข้าไปใน prompt ของผู้ใช้ แล้วส่งไปยังโมเดล **text generation** ที่ host อยู่บน Nutanix Enterprise AI

5.  **Get Answer**
    
    -   chatbot ส่งคำตอบกลับไปให้ผู้ใช้

## Steps to Create a RAG-enabled Chatbot

ในการ chat กับเอกสารและข้อมูลของคุณ คุณจะทำสิ่งต่อไปนี้:

1.  **Upload a document to Nutanix Objects**
    
    -   เข้าถึง object store bucket ของคุณและอัปโหลดเอกสารตัวอย่างเข้าไป

2.  **Configure a Document Store in Flowise**
    
    -   กำหนดค่า document store ที่ชี้ไปยัง bucket ของคุณ และกำหนดวิธีแบ่งและประมวลผลเอกสาร

3.  **Upsert the document chunks into a Vector Database**
    
    -   กำหนดค่า vector database และข้อมูล embedding model เพื่อสร้าง vector จากเอกสารของคุณ
    
4.  **Configure existing chatflow to use RAG**
    
    -   ใน chatflow ของคุณ คุณจะเพิ่ม node ใหม่สำหรับ Document Store และปรับแต่ง node **Conversation Chain** เพื่อเชื่อมต่อกับ Document Store ของคุณ

---

[← Back: Configure Shared Endpoint](nai-application-chatbot-shared.md) | [Home](nai-welcome.md) | [Next: Obtain Object Store URL and Credentials →](nai-application-rag-store.md)