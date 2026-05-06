# Capacity Planning

## Overview

ในส่วนนี้ คุณจะได้เรียนรู้วิธีการใช้ฟีเจอร์ Capacity Forecast and Planning เพื่อรับ hardware recommendations ในการขยาย capacity runway และวางแผนสำหรับ workloads ในอนาคต

Nutanix Cloud Manager มาพร้อมกับ built-in machine learning (เรียกว่า X-Fit และออกเสียงว่า cross fit) ซึ่งจะคอยวิเคราะห์และเรียนรู้อย่างต่อเนื่องว่า running workloads มีการใช้งาน (consume) ตัว compute และ storage capacity อย่างไร ตัว machine learning นี้จะให้ actionable signals ที่ admin สามารถนำไปดำเนินการต่อได้ หนึ่งใน signals ที่ให้มาก็คือตัวเลข “cluster runway” ซึ่งจะพยากรณ์ว่า existing capacity จะสามารถตอบสนองต่อความต้องการของธุรกิจได้ดีแค่ไหน โดยพิจารณาจาก predicted growth rates ที่คำนวณจาก past growth patterns

ตัว engine เดียวกันนี้ยังช่วยให้ admins สามารถวางแผนสำหรับ future expansion ได้ โดยอนุญาตให้ users สามารถสร้าง planning scenarios ตามความต้องการของพวกเขาได้ ในทั้งสองกรณี (เพื่อตอบสนองต่อ existing runway needs หรือเพื่อวางแผนสำหรับ future workloads) Nutanix Cloud Manager ได้จัดเตรียมเครื่องมือที่ใช้งานง่ายแต่ทรงพลัง ที่จะให้ hardware recommendations เพื่อตอบสนองความต้องการของธุรกิจ

![](/images/capacityplanning1.ae506819.png)

## Walkthrough Demo

นี่คือ [linkopen in new window](https://nutanix.storylane.io/share/scmtpldnvhqi) สำหรับ guided walkthrough เพื่อสาธิตให้เห็นการทำงานของ feature นี้ เมื่อคุณทำ guided walk-through นี้เสร็จแล้ว ให้กลับมาที่ lab manual
