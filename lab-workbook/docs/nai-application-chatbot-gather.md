# Gather Information

คุณจะต้องมีข้อมูล 3 อย่างเพื่อเริ่มต้น:

-   URL ของ endpoint ที่คุณสร้างขึ้น
-   ชื่อ endpoint
-   API key ที่มีสิทธิ์เข้าถึง endpoint นั้น

หากต้องการ API key ใหม่ ให้ทำตามคำแนะนำใน [Creating Additional API Keys](nai-fundamentals-endpoint-keys.md)

ข้อมูลอื่นๆ สามารถดูได้จากหน้า endpoint dashboard จาก Nutanix Enterprise AI interface คลิก Endpoints แล้วคลิกชื่อ endpoint เพื่อดู endpoint dashboard ค้นหาค่าต่อไปนี้:

-   Details > Endpoint Name
-   Endpoint Access > Endpoint URL

![Endpoint Dashboard](images/endpoint-dashboard.a187d44f.png)

!!! warning
    อย่า include `chat/completions` เมื่อ copy URL Flowise ต้องการเพียง base URL path เท่านั้น

---

[← Back: Create Your First Chatbot](nai-application-chatbot.md) | [Home](nai-welcome.md) | [Next: Create a Blank Chatflow in Flowise →](nai-application-chatbot-flowise.md)