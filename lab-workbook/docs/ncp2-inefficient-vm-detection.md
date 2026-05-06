# VM Inefficiency

## Overview

ในส่วนนี้ คุณจะได้เรียนรู้เกี่ยวกับฟีเจอร์ Resource Inefficiency Detection และวิธีดู inefficient VMs ภายใน UI

อัลกอริทึม machine learning อย่าง X-Fit จะดู historical data ของ VM เพื่อหา expected baseline และ future prediction โดยคำนึงถึงรูปแบบ seasonality ข้อมูล baseline และ prediction จะถูกนำมารวมกับ usage ในช่วง 21 วันที่ผ่านมาเพื่อพิจารณา expected usage สำหรับ VM และจะทำเครื่องหมาย (mark) VM ว่า inefficient ตาม thresholds ที่กำหนดไว้ ตัวอย่างเช่น หาก average memory usage ของ VM ในช่วง 21 วันที่ผ่านมาน้อยกว่า 20% ในส่วนใหญ่ของเวลา (>99.5% ของเวลา) VM นั้นจะถูกทำเครื่องหมายว่า overprovisioned ในส่วนของ memory

ในส่วนก่อนหน้านี้ เราได้ทำการ extend ตัว runway โดยรับ hardware recommendation อีกวิธีหนึ่งในการ extend ตัว runway คือการ optimize ตัว current workloads โดยจัดการกับ inefficiencies ต่างๆ

ประเภทของ inefficiency ทั้ง 4 ประเภทได้แก่:

-   Overprovisioned - VM ที่มีขนาดใหญ่เกินไป (over-sized) และทำให้สิ้นเปลือง resources ที่ไม่จำเป็น
-   Constrained - VM ที่มี resources ไม่เพียงพอต่อ demand และอาจนำไปสู่ performance bottlenecks ได้
-   Bully - VM ที่ consume ตัว resources มากเกินไปและทำให้ VMs อื่นๆ ขาดแคลน (starve)
-   Inactive - VM ที่ถูก powered off หรือมี I/O และ network traffic น้อยมาก (minimal)

![](/images/inefficientvms1.fe062f9c.png)

## Interactive Demo

คลิกที่ลิงก์นี้

👀 [Guided walkthrough of VM right sizing.open in new window](https://nutanix.storylane.io/share/1cyx8cigr5qk) 👀

นี่คือ [linkopen in new window](https://nutanix.storylane.io/share/1cyx8cigr5qk) ไปยัง guided walkthrough เพื่อสาธิตการทำงานของ feature นี้ เมื่อคุณทำ guided walk-through นี้เสร็จสิ้นแล้ว ให้กลับมาที่ lab manual
