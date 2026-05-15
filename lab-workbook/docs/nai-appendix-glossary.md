# Glossary

คำศัพท์และเทคโนโลยีที่คุณจะพบใน lab นี้

### Embedding

Embedding คือการแสดงแทนเชิงตัวเลขของวัตถุสำหรับใช้ในระบบ machine learning ซึ่งยังคงมูลค่าเชิงความหมายไว้ ช่วยให้แอปพลิเคชันสามารถระบุความสัมพันธ์ระหว่างข้อมูลสองชิ้นได้ เช่นเดียวกับโมเดลพื้นฐานที่ใช้สำหรับ chatbot (เช่น LLama2) มี embedding model ที่ผ่านการ pre-trained มากมายเพื่อช่วยในการแปลนี้ โมเดลเหล่านี้ได้เรียนรู้จากข้อความจำนวนมหาศาลเพื่อทำความเข้าใจว่าคำต่างๆ ถูกใช้อย่างไรในบริบทใด

Embedding และ embedding model เป็น component สำคัญของ Retrieval Augmented Generation pipeline

### Flowise

Flowise เป็น low-code tool ที่ช่วยให้นักพัฒนาสามารถสร้าง LLM orchestration flow และ AI agent แบบ custom บน LangChain และ/หรือ LlamaIndex โดย LangChain และ LlamaIndex เป็น framework สำหรับลดความซับซ้อนในการพัฒนาแอปพลิเคชัน AI

### Milvus

[Milvusopen in new window](https://milvus.io/) เป็น open-source vector database เป็น component สำคัญใน RAG pipeline ที่จัดเก็บ embedding ทั้งหมดที่สร้างโดย embedding model

Milvus รองรับ backend storage หลายประเภท รวมถึง Nutanix Objects

### Retrieval Augmented Generation (RAG)

Retrieval Augmented Generation เป็นเทคนิคในการดึงข้อมูลจากภายนอก large language model เพื่อเพิ่มประสิทธิภาพ prompt ของคุณ กล่าวง่ายๆ คือ เมื่อคุณ query LLM (เช่น การถามคำถาม ChatGPT) โมเดลจะให้คำตอบตามข้อมูลที่ใช้ฝึกโมเดล Retrieval Augmented Generation หรือ RAG เป็นเทคนิคที่โมเดลจะ query content store ก่อนเพื่อดึงข้อมูลที่เกี่ยวข้องกับ input ก่อนให้คำตอบ

---

[← Back: Conclusion](nai-conclusion.md) | [Home](nai-welcome.md) | [Next: Flowise Chatflows →](nai-appendix-flowise.md)