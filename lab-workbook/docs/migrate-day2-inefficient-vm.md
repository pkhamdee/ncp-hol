# VM Inefficiency

## Overview

ในส่วนนี้ คุณจะได้เรียนรู้เกี่ยวกับฟีเจอร์ Resource Inefficiency Detection และวิธีการดู inefficient VMs ภายใน UI

ตัว X-Fit machine learning algorithm จะดู historical data ของ VM เพื่อหา expected baseline และ future prediction โดยคำนึงถึง seasonality patterns ข้อมูล baseline และ prediction จะถูกนำมารวมกับ usage ในช่วง 21 วันที่ผ่านมาเพื่อพิจารณา expected usage สำหรับ VM และจะ mark ตัว VM ว่าเป็น inefficient ตาม thresholds ที่กำหนดไว้ ตัวอย่างเช่น หาก average memory usage ของ VM ในช่วง 21 วันที่ผ่านมาน้อยกว่า 20% เป็นส่วนใหญ่ (มากกว่า 99.5% ของเวลาทั้งหมด) VM นั้นจะถูก mark ว่า overprovisioned ในส่วนของ memory

ในส่วนก่อนหน้านี้ เราได้ทำการขยาย runway โดยการรับ hardware recommendation อีกวิธีหนึ่งในการขยาย runway คือการ optimize ตัว current workloads โดยจัดการกับเรื่อง inefficiencies ต่างๆ

ประเภทของ inefficiency 4 ประเภท ได้แก่:

-   Overprovisioned - VM ที่มีขนาดใหญ่เกินไป (over-sized) และสิ้นเปลือง resources ที่ไม่จำเป็น
-   Constrained - VM ที่มี resources ไม่เพียงพอต่อความต้องการและสามารถนำไปสู่ปัญหา performance bottlenecks ได้
-   Bully - VM ที่ใช้ resources มากเกินไปและทำให้ VMs อื่นๆ ขาดแคลน (starve)
-   Inactive - VM ที่ถูก powered off หรือมี I/O และ network traffic น้อยมาก

![](/images/inefficientvms1.fe062f9c.png)

รายละเอียดเพิ่มเติมเกี่ยวกับวิธีการที่ VMs ถูก profiled สามารถอ่านได้ใน [documentation](https://portal.nutanix.com/page/documents/details?targetId=Intelligent-Operations-Guide-vpc_2024_3:mul-behavioral-learning-pc-c.html)

## Walkthrough

นี่คือ [link](https://nutanix.storylane.io/share/1cyx8cigr5qk) สำหรับ guided walkthrough เพื่อสาธิตให้เห็นการทำงานของ feature นี้ เมื่อคุณทำ guided walk-through เสร็จแล้ว ให้กลับมาที่ lab guide นี้
