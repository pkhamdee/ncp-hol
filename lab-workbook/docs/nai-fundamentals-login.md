# Log in to Nutanix Enterprise AI

## Accessing the shared Nutanix Enterprise AI instance

เริ่มตั้งแต่ lab นี้ คุณจะใช้ Nutanix Enterprise AI instance ที่เตรียมไว้และใช้ร่วมกับผู้ใช้คนอื่น คุณได้รับ role `AI/ML User`

1.  เข้าถึง Nutanix Enterprise AI console ที่ใช้ร่วมกันในแท็บใหม่
    
    !!! tip    
        รายละเอียดการเชื่อมต่อสำหรับ instance ที่ใช้ร่วมกันสามารถดูได้จากหน้า [Setup](nai-intro-setup.md) ของ lab guide นี้
    
2.  เข้าสู่ระบบด้วย credentials ที่ได้รับมอบหมาย
    
    ![NAI Console](images/nai-console.4473f4d4.png)
    
    !!! info
        AI/ML User
        
        การเข้าสู่ระบบด้วย role `AI/ML user` จะมีมุมมองที่จำกัดกว่า role `AI/ML Admin`
    
3.  คลิก **Settings**
    
4.  สังเกตว่า Hugging Face credential ได้รับการตั้งค่าล่วงหน้าให้คุณแล้ว จำเป็นต้องมี Hugging Face token ที่ถูกต้องสำหรับการดาวน์โหลดโมเดลจาก Hugging Face
    
    !!! info
        Third Party Credentials
        
        Third Party Credentials มีความเฉพาะสำหรับผู้ใช้แต่ละคนที่เข้าสู่ระบบ
    
    !!! info
        NVIDIA NGC Personal Key
        
        **NVIDIA NGC Personal Key** จำเป็นสำหรับการดาวน์โหลดโมเดล NVIDIA NIM อย่างไรก็ตาม เราจะไม่ใช้มันใน lab นี้

---

[← Back: Review the NAI Interface](nai-fundamentals-interface.md) | [Home](nai-welcome.md) | [Next: Import an LLM →](nai-fundamentals-import-llm.md)