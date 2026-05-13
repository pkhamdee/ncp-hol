# Nutanix Cloud Platform

## Overview

Nutanix Self-Service ช่วยให้คุณสามารถ select, provision, และ manage ตัว business applications ของคุณได้อย่างราบรื่นทั่วทั้ง infrastructure ของคุณสำหรับทั้ง private และ public clouds

Infrastructure-as-a-Service (IaaS) ถูกกำหนดให้เป็นความสามารถในการจัดหา compute resources ได้อย่างรวดเร็วแบบ on-demand ผ่าน self-service portal ในขณะที่ลูกค้าหลายรายใช้ Nutanix Self-Service (ก่อนหน้านี้คือ Calm) เพื่อ orchestrate ตัว applications ที่มีความซับซ้อนแบบ multi-tiered แต่ลูกค้าส่วนใหญ่ก็ยังใช้ Self-Service เพื่อจัดเตรียม basic IaaS ให้กับ end users ของพวกเขาด้วย

ใน lab นี้ คุณจะได้สร้าง Single VM Blueprint ที่เป็น Linux และ Windows (ทางเลือก), ทำการ define ตัว variables และ publish ตัว blueprints ไปยัง Marketplace

-   [Create Linux Blueprint](selfservice-linux.md)
-   (Optional) [Create Windows Blueprint](selfservice-windows.md)
    -   การสร้าง Windows Blueprint เป็นทางเลือก (optional) การทำ Provisioning เฉพาะ Linux Blueprint ก็เพียงพอสำหรับการทำขั้นตอนต่อๆ ไปใน lab นี้ให้เสร็จสมบูรณ์
