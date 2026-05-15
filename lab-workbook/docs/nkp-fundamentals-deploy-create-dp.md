# Using kubectl to Create a Deployment

มาลอง deploy แอปแรกของเราบน Kubernetes ด้วยคำสั่ง `kubectl create deployment` กันเถอะ

1.  เราจำเป็นต้องระบุชื่อ deployment และตำแหน่งของ app image กลับไปที่ VS Code terminal ให้รันคำสั่งต่อไปนี้ โดยอัปเดต `##` เป็นหมายเลข user ของคุณก่อน
    
    -   command

    ```
    kubectl create deployment user##-nkp-simple-app --image=nginx:1.27
    ```
    
    -   example

    ```
    kubectl create deployment user01-nkp-simple-app --image=nginx:1.27
    ```
    
    -   output (ตัวอย่าง)

    ```
    deployment.apps/user01-nkp-simple-app created
    ```
    
    🎉 เยี่ยมมาก! คุณเพิ่งจะ deploy แอปพลิเคชันแรกของคุณสำเร็จด้วยการสร้าง deployment
    
    **(ทางเลือก)** คำอธิบายเกี่ยวกับการสร้าง deployment
    
    สิ่งนี้ช่วยดำเนินการบางอย่างให้คุณดังนี้:
    
    -   ค้นหา node ที่เหมาะสมที่สามารถรัน instance ของแอปพลิเคชันได้
    -   ทำการ schedule ให้แอปพลิเคชันรันบน Node นั้น
    -   ตั้งค่า (configure) คลัสเตอร์ให้ทำการ reschedule ตัว instance บน Node ใหม่เมื่อจำเป็น
    
2.  ดำเนินการต่อเพื่อดูรายการ (list) ของ deployments:
    
    -   command

    ```
    kubectl get deployments
    ```
    
    -   output (ตัวอย่าง)

    ```
    NAME                                                      READY   UP-TO-DATE   AVAILABLE   AGE
    cluster-autoscaler-0193785b-4589-7210-9427-7709ece7adfc   1/1     1            1           45h
    user01-nkp-simple-app                                     1/1     1            1           30s
    ```
    
    เราจะเห็นว่า deployment `user##-nkp-simple-app` กำลังรัน app จำนวนหนึ่ง instance
    

#### View the app

เราจะกล่าวถึงตัวเลือกอื่นๆ ในการ expose แอปพลิเคชันของคุณออกสู่ภายนอก Kubernetes cluster ใน lab ถัดไป

**(ทางเลือก)** การดูแอปพลิเคชันระหว่างการพัฒนา (development)

Pods ที่กำลังรันอยู่ภายใน Kubernetes นั้นจะรันอยู่บนเครือข่ายส่วนตัว (private network) ที่ถูกแยกออกมา โดยค่าเริ่มต้น (By default) พวกมันจะสามารถมองเห็นได้จาก pods และ services อื่นๆ ภายใน Kubernetes cluster เดียวกันเท่านั้น แต่จะไม่สามารถมองเห็นได้จากภายนอกเครือข่ายนั้น

คำสั่ง `kubectl proxy` สามารถสร้าง proxy ที่จะ forward การสื่อสารเข้าไปยังเครือข่ายส่วนตัวทั่วทั้งคลัสเตอร์ได้ เราจะไม่ใช้ตัวเลือกนี้เนื่องจากมีประโยชน์เฉพาะในระหว่างการพัฒนา (development) เท่านั้น


---

[← Back: Accessing the shared NKP environment](nkp-fundamentals-deploy-access.md) | [Home](nkp-bootcamp.md) | [Next: Using a Service to Expose Your App →](nkp-fundamentals-deploy-expose.md)