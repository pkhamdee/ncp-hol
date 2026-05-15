# View the Endpoint Details

นอกจากจะช่วยให้คุณทดสอบ endpoint ได้แล้ว endpoint dashboard ยังแสดงข้อมูลที่เป็นประโยชน์เกี่ยวกับ endpoint ของเรา ได้แก่:

-   Endpoint Details
-   Endpoint URL และ API keys
-   Compute instances ที่กำลังรัน
-   API Requests Summary
-   Token Usage

![Endpoint Dashboard](images/endpoint-dashboard.8ff11768.png)

1.  เมื่อ endpoint ถูกใช้งานอยู่ คุณสามารถดู metrics ที่ละเอียดยิ่งขึ้นได้ในหน้า Metrics คลิก **Metrics > Usage** และ **Metrics > Performance** เพื่อดูว่า metrics ประเภทใดที่มีให้ใช้งาน

    ![Endpoint Metrics](images/endpoint-tabs-metrics.d087df83.png)

2.  คลิก **Logs** เพื่อดู log ของ endpoint นี่คือ Kubernetes pod log ซึ่งช่วยในการแก้ไขปัญหาเบื้องต้นโดยไม่จำเป็นต้องเข้าถึง Kubernetes environment โดยตรง
    
    ![Endpoint Logs](images/endpoint-tabs-logs.72e74f31.png)
    
    ![Viewing Endpoint Logs](images/endpoint-logs.0710fbc9.png)

---

[← Back: Test the Endpoint](nai-fundamentals-endpoint-test.md) | [Home](nai-welcome.md) | [Next: Creating Additional API Keys →](nai-fundamentals-endpoint-keys.md)