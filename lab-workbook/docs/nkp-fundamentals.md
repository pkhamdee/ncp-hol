# Fundamentals: Basic NKP concepts

**วัตถุประสงค์:** บทนี้จะช่วยเตรียมทักษะพื้นฐานสำหรับการทำงานกับ NKP environments ให้กับคุณ เมื่อทำภารกิจเหล่านี้เสร็จสิ้น คุณจะได้รับข้อมูลเชิงลึกในเชิงปฏิบัติเกี่ยวกับ core NKP features [cite: nkp-fundamentals.md]

**สิ่งที่คุณจะได้ทำ:**

1.  **NKP quick tour**
    
    -   แม้ว่าคุณจะเคยเห็น NKP console มาก่อน แต่เราขอแนะนำให้คุณทำ quick tour นี้เพื่อให้คุ้นเคยกับ layout และเพื่อให้ค้นหา resources ที่คุณกำลังมองหาได้รวดเร็วยิ่งขึ้น [cite: nkp-fundamentals.md]

2.  **Deploy and expose a simple application**
    
    -   Launch ตัว basic app แรกของคุณบน Kubernetes และใช้งาน kubectl! [cite: nkp-fundamentals.md]
    -   คุณจะได้เรียนรู้วิธีใช้ Kubernetes services เพื่อ expose ตัว application ของคุณสำหรับการ testing [cite: nkp-fundamentals.md]

3.  **Expose the sample application on production**
    
    -   Service type _NodePort_ นั้นไม่ reliable หรือ scalable พอสำหรับ production มาเปลี่ยนไปใช้ `Ingress` และ `LoadBalancer` กันแทน [cite: nkp-fundamentals.md]

4.  **Preparing for multi-tenancy**
    
    -   Dedicated Kubernetes clusters (hard multi-tenancy) อาจมีราคาสูง อีกทางเลือกหนึ่งคือการ share clusters (soft multi-tenancy) ระหว่างหลายๆ ทีม [cite: nkp-fundamentals.md]

5.  **Persistent storage**
    
    -   Persistent storage มีความสำคัญใน Kubernetes สำหรับการจัดการ stateful workloads ซึ่งช่วยให้ data ยังคงอยู่ (persist) ได้แม้จะมีการ restarts ตัว pod ก็ตาม [cite: nkp-fundamentals.md]
    
6.  **Recap and next steps**
    
    -   ตอนนี้คุณได้เรียนรู้เพิ่มเติมเกี่ยวกับ core NKP features แล้ว คุณจะพร้อมสำหรับการค้นพบ key features บางส่วนที่ NKP มีให้สำหรับการจัดการ day-2 operations สำหรับ infrastructure และ applications [cite: nkp-fundamentals.md]
