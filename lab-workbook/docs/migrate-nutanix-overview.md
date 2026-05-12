# Migrate to Nutanix

กำลังพิจารณาเรื่อง migration จากสภาพแวดล้อมเดิมไปยัง Nutanix อยู่หรือไม่? กังวลว่าการเปลี่ยนผ่านระบบจะยุ่งยากแค่ไหนใช่ไหม? Nutanix ช่วยให้กระบวนการ migration ง่ายขึ้นภายในไม่กี่ขั้นตอน เมื่อถึงเวลาที่ต้องทำการ migration ไปยัง Nutanix ผ่านหนึ่งในเส้นทางที่รองรับด้านล่าง เครื่องมือ **Nutanix Move** จะจัดการการเปลี่ยนผ่านระบบและลดความเสี่ยงของการหยุดชะงัก (disruption) หรือค่าใช้จ่ายที่สูงในการสร้าง virtual machines ขึ้นมาใหม่

Move เป็นโซลูชัน cross-hypervisor mobility ที่รองรับเส้นทางการ migration มากมาย ได้แก่:

-   Nutanix AHV to Nutanix AHV
-   Nutanix AHV to AWS EC2
-   Nutanix AHV to Microsoft Azure Cloud
-   Nutanix AHV to NC2 on AWS/Azure
-   VMware ESXi (legacy infrastructure or Nutanix) to AHV
-   VMware ESXi (legacy infrastructure or Nutanix) to VMware ESXi on Nutanix
-   VMware ESXi to Nutanix Cloud Clusters (NC2) on AWS
-   VMware ESXi to NC2 on Microsoft Azure
-   VMware Cloud (VMC) on AWS to AHV
-   VMware Cloud (VMC) on AWS to NC2 on AWS

Additional Migration Paths

-   Microsoft Hyper-V to AHV
-   Microsoft Hyper-V to VMware ESXi on Nutanix
-   Microsoft Hyper-V to NC2 on AWS
-   AWS EC2 to AHV
-   AWS EC2 to VMware ESXi on Nutanix
-   AWS EC2 to NC2 on AWS
-   Microsoft Azure Cloud to AHV
-   Microsoft Azure Cloud to VMware ESXi on Nutanix
-   Microsoft Azure Cloud to NC2 on Azure
-   NC2 on AWS/Azure to Nutanix AHV
-   NC2 on Azure to NC2 on Azure

ในช่วง 1-2 ชั่วโมงข้างหน้านี้ hands-on lab นี้จะพาคุณไปสัมผัสประสบการณ์การ migration VMs จากสภาพแวดล้อม on-prem Nutanix ไปยังสภาพแวดล้อม on-prem ที่รัน Nutanix AHV และ Nutanix Cloud Platform นอกจากนี้เราจะใช้ Move เพื่อทำ migration ตัว file shares ไปยังสภาพแวดล้อม Nutanix Files ด้วย

## Learning Outcomes

เมื่อจบเซสชันนี้ คุณจะสามารถ:

-   เรียนรู้วิธีการเข้าถึงและวางแผนสำหรับการพูดคุยเรื่อง migration ได้อย่างมั่นใจ
-   เรียนรู้การ migration VMs และ workloads ไปยัง AHV โดยใช้ Move
-   เรียนรู้เทคนิคการ migration ขั้นสูงด้วย Move โดยใช้ categories และ policies
-   เรียนรู้การ migration ตัว file shares ไปยัง Nutanix Files โดยใช้ Move เพื่อรวบรวม (consolidate) infrastructure เข้าด้วยกัน
-   เรียนรู้การจัดการ day 2 management บน Nutanix ซึ่งประกอบด้วย:
    -   เรียนรู้วิธีการและเหตุผลที่ต้องติดตั้ง Nutanix Guest Tools บน VMs
    -   Identity and Access Management (IAM)
    -   วิธีการใช้ X-Play สำหรับ no-code automation
    -   การตรวจหา inefficient VMs
-   ท้ายที่สุด เข้าใจว่ามี resources และ tools ใดบ้างที่สามารถนำมาใช้ในการวางแผนและดำเนินการ migration ให้สำเร็จ
