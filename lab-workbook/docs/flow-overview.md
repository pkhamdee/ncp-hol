# Flow Network Security

## Overview

Flow Network Security (FNS) มอบความปลอดภัยโดยใช้ microsegmentation ร่วมกับ distributed firewall สำหรับ VMs บน Nutanix hypervisor, AHV. FNS ผสมผสาน centralized management ผ่าน Prism Central เข้ากับ distributed enforcement ในทุกๆ nodes. มันมอบการทำ application-centric policy management โดยใช้ logical grouping ที่เรียกว่า Categories, มี visualization ที่หลากหลาย, และ automation ด้วย APIs และ NCM Self-Service

Flow Network Security Next-Gen สร้างขึ้นบนพื้นฐานของ Network Controller (Flow Controller ใน NCI 7.5) เพื่อมอบ enhanced policy-model, increased scale, ฟีเจอร์เพิ่มเติม และรองรับทั้ง VLAN และ VPC networks

## What's New

labs นี้ถูกเขียนขึ้นสำหรับซอฟต์แวร์เวอร์ชันต่อไปนี้ซึ่งเป็นส่วนหนึ่งของ NCI 7.5 release:

-   Prism Central - 7.5
-   AOS - 7.5
-   AHV - 11.0
-   Flow Network Security - 7.5
-   Flow Controller - 7.5

lab เวอร์ชันนี้แสดงให้เห็นถึง capabilities ของ Flow Network Security Next-Gen เราจะเจาะลึกไปที่ capabilities ใหม่ๆ ของ updated policy-model เช่น การทำ clone, save, และ multi-environment isolation policies
