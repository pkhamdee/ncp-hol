# Expose app on production Lab

ตอนนี้ application ของเรายังไม่ถูก expose ในวิธีที่ reliable และ scalable ตัว Worker nodes ถือว่าเป็นสิ่งชั่วคราว (ephemeral) หาก node ถูก redeploy และ address เปลี่ยนไป ตัว application จะไม่สามารถเข้าถึงได้ผ่าน node นั้น

ใน lab นี้ คุณจะได้:

-   Migrate ตัว application service ของคุณเป็น `LoadBalancer` ด้วย **MetalLB**
-   Expose ตัว HTTP applications โดยใช้ `Ingress` ด้วย **Traefik**
