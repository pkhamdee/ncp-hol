# Definitions

ก่อนจะเริ่มทำ lab นี้ เรามาทำความเข้าใจคำศัพท์สำคัญบางประการเกี่ยวกับ business continuity และ disaster recovery กันก่อน 

## What is a Recovery Point Objective (RPO)?

RPO คือการวัดปริมาณข้อมูลที่สูญหายซึ่งธุรกิจยอมรับได้สำหรับ application หรือกระบวนการทางธุรกิจ 

ตัวอย่างเช่น ธุรกิจอาจระบุว่า "application นี้มี RPO ไม่เกินหนึ่งชั่วโมง" นั่นหมายความว่าต้องมีการป้องกันไว้ (snapshots, backups, synchronous replication หรือ application level High Availability) เพื่อให้คุณสามารถกู้คืนข้อมูลได้โดยสูญเสียข้อมูลไม่เกิน RPO สูงสุดที่หนึ่งชั่วโมง 

การสูญเสียข้อมูลน้อยกว่าหนึ่งชั่วโมงถือว่ายอมรับได้ แต่การสูญเสียข้อมูลมากกว่าหนึ่งชั่วโมงถือว่ายอมรับไม่ได้ 

## What is a Recovery Time Objective (RTO)?

RTO คือระยะเวลาที่ใช้ในการกู้คืนจากเหตุการณ์ขัดข้อง โดยปกติ RTO จะถูกแบ่งย่อยสำหรับการกู้คืนแต่ละ application หรือกระบวนการทางธุรกิจ แต่ก็อาจรวมถึง RTO สูงสุดในการกู้คืน applications และ services ทั้งหมดทั่วทั้งธุรกิจด้วย 

## What is a Category?

category คือโครงสร้างใน Prism Central ที่ใช้คู่ key-value เพื่อระบุฉลาก แยกประเภท หรือจัดหมวดหมู่กลุ่มของ VMs คุณสามารถคิดเสียว่าเป็น tags หรือป้ายกำกับที่นำไปใช้กับ entities คุณสามารถเชื่อมโยง entities เช่น VMs, VGs และ CGs เข้ากับ disaster recovery policy ได้อย่างง่ายดายผ่าน categories 

การกำหนด policy จะอ้างอิงถึง category โดยที่ entities ทั้งหมดจะถูกเลือกเข้าไปใน policy เนื่องจากพวกมันถูกจัดหมวดหมู่ไว้อย่างเหมาะสม 

## What is a Protection Policy?

Protection Policy คือโครงสร้างสำหรับกำหนด Recovery Point Objective (RPO) รวมถึงสถานที่หรือกลุ่มสถานที่ที่จะทำการ replicate snapshots 

## What is a Recovery Plan?

Recovery Plan คือโครงสร้างที่เราใช้ในการกำหนดว่า entities ที่ได้รับการปกป้องควรจะถูกกู้คืนอย่างไร ซึ่งรวมถึง boot stages, script execution และ network mapping 

## What is Entity-Centric DR or Nutanix Disaster Recovery?

Entity-centric DR หรือ Nutanix Disaster Recovery ทั้งสองเป็นคำที่ใช้เรียกการจัดการ DR บน Virtual Machines (VMs), Volume Groups (VGs) และ Consistency Groups (CGs) จาก Prism Central ซึ่งประกอบด้วย runbook automation, boot stages และ network mapping เนื่องจาก policies ถูกนำไปใช้กับ "entities" แต่ละตัวผ่าน categories จึงเรียกว่า entity-centric ซึ่งต่างจากการดำเนินการที่ทำกับ storage container ทั้งหมด 

## Next Steps

เมื่อคุณเข้าใจคำศัพท์สำคัญใน Nutanix Disaster Recovery แล้ว เรามาไปที่ส่วนถัดไปกันเลย 

[← Back: Overview](edge-lab-scenario3-overview.md) | [Home](edge-getting-started.md) | [Next: Setup →](edge-lab-scenario3-setup.md)
