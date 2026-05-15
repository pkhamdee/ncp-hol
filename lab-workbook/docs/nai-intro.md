# Introduction

ใน lab นี้ คุณจะใช้งาน cluster Nutanix Cloud Platform แบบ 4-node ที่ใช้ร่วมกัน ซึ่งรัน Nutanix Kubernetes Platform, Nutanix Unified Storage และ Nutanix Enterprise AI

![](images/NAI_Stack.bd38a9af.png)

คุณจะใช้ Nutanix Enterprise AI เพื่อดูว่าการเชื่อมต่อกับ model repository ต่างๆ เพื่อดาวน์โหลด LLM การสร้าง endpoint ที่ปลอดภัย และการเชื่อมต่อแอปพลิเคชัน GenAI กับ endpoint นั้นทำได้ง่ายเพียงใด คุณจะสร้าง chatbot ของตัวเองแล้วเชื่อมต่อกับ database เพื่อเพิ่มประสิทธิภาพ chatbot ด้วยเอกสารส่วนตัว

!!! note

    เนื่องจาก lab นี้มีการใช้งานร่วมกัน คุณจะทำงานกับโมเดล Llama3-1B ของตัวเองบน Nutanix Enterprise AI **instance** ที่ใช้ร่วมกัน โดยใช้ CPU เท่านั้น

    เมื่อสร้างแอปพลิเคชัน RAG (ในส่วน Application) คุณจะใช้ Nutanix Enterprise AI **inference endpoint** ที่ใช้ร่วมกัน ซึ่งรันโมเดล Llama3-8B โดยมี NVIDIA L40S GPU รองรับ

---

[← Back: Nutanix Enterprise AI](nai-welcome.md) | [Home](nai-welcome.md) | [Next: Setup →](nai-intro-setup.md)