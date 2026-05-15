# Create an Endpoint and API Key

1.  จากเมนูทางซ้าย คลิก **Endpoints**
    
    ![Endpoints](images/menu-endpoint.328f21e7.png)
    
2.  คลิก **Create New Endpoint**
    
    ![Create New Endpoint](images/create-new-endpoint.4f696eb6.png)
    

จะมีหน้าจอสั้นๆ 3 ขั้นตอนให้ผ่าน:

-   Basics
-   Configuration
-   Summary

## Basics

1.  ตั้งชื่อ endpoint ว่า `llama-endpoint##` โดยที่ `##` ตรงกับ username ของคุณ
    
2.  เลือกโมเดลของคุณจาก drop-down list **Model Instance Name** โดยจะแสดงเฉพาะโมเดลที่มีสถานะ Active ซึ่ง import โดย user ของคุณเท่านั้น
    
3.  ใต้ **Acceleration Type** เลือก CPU
    
4.  คลิก **Create a New API Key** เพื่อสร้าง API key
    
    ![Create API Key Link](images/create-api-key-from-ep0.5b04a118.png)
    
5.  ตั้งชื่อ API Key แล้วคลิก **Create**
    
    ![Create API Key](images/create-api-key-from-ep.f1493f84.png)
    
6.  คลิก Copy API Key
    
    ![Copy API Key](images/copy-api-key.51ed8d3e.png)
    
    !!! tip    
        อย่าลืม copy API key ไปไว้ใน text editor (เช่น VS Code) ใน VDI session ของคุณเพื่อใช้ในภายหลัง
    

## Configuration

1.  เราจะปล่อยให้ค่า default ของ compute configuration ไว้ก่อน
2.  คลิก **Next**

!!! note

    inference engine กำหนด library ที่ใช้สำหรับ LLM inferencing และ serving สำหรับ CPU-based inference จะใช้ open-source vLLM engine เสมอ สำหรับ GPU-based inference สามารถใช้ TGI หรือ NIM ได้ด้วย:

    -   **TGI (Text Generation Interface)** - toolkit สำหรับ inference ของ Hugging Face ซึ่งอยู่ใน maintenance mode ตั้งแต่เดือนธันวาคม 2025
    -   **NVIDIA NIM** - เมื่อดาวน์โหลด NVIDIA NIMs คุณกำลังดาวน์โหลด NIM (NVIDIA Inference Microservices) container ซึ่งจะถูกรันเมื่อเปิด endpoint ดังนั้นต้องเลือก NVIDIA NIM engine เมื่อใช้งานโมเดล NVIDIA

## Summary

1.  ตรวจสอบหน้า summary ควรมีลักษณะคล้ายกับภาพด้านล่าง หากต้องการแก้ไขอะไร สามารถคลิก **Back** ได้
    
    ![Sumary Screen](images/tab-summary.840efeba.png)
    
2.  คลิกปุ่ม **Create** เมื่อพร้อม
    
3.  endpoint จะถูก provision รอจนกว่าสถานะจะเป็น **Active** ก่อนไปขั้นตอนถัดไป (ประมาณ 3-4 นาที)
    
    ![Active Endpoint](images/active.9e35c208.png)
    
!!! info
    สิ่งที่เกิดขึ้นใน Backend

    pod กำลังถูก schedule บน Kubernetes cluster ใน backend พร้อม vCPU และ Memory ที่กำหนดไว้ในระหว่างการสร้าง endpoint (8vCPU/12GB) pod นี้จะรัน inference service จำนวน instance ที่กำหนดใน endpoint คือจำนวน pod replica ที่จะถูกสร้าง เนื่องจากข้อจำกัดของ lab environment ให้ใช้ค่า default 1 instance

---

[← Back: Create an Endpoint Overview](nai-fundamentals-endpoint.md) | [Home](nai-welcome.md) | [Next: Test the Endpoint →](nai-fundamentals-endpoint-test.md)