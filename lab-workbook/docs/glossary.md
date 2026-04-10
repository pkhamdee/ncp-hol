# Appendix

# [#](#glossary) Glossary

## [#](#nutanix-core) Nutanix Core

### [#](#aos) AOS

AOS ย่อมาจาก Acropolis Operating System และเป็นระบบปฏิบัติการที่รันบน Controller VM (CVM)

### [#](#pulse) Pulse

Pulse ให้ข้อมูลระบบวินิจฉัยแก่ทีม customer support ของ Nutanix เพื่อให้สามารถให้การสนับสนุนที่เชิงรุกและตระหนักถึงบริบทสำหรับ Nutanix solutions

### [#](#prism-element) Prism Element

Prism Element เป็น management plane ดั้งเดิมสำหรับ Nutanix เนื่องจากการออกแบบอิงตาม interface ผลิตภัณฑ์สำหรับผู้บริโภค จึงใช้งานง่ายและง่ายกว่า interface แอปพลิเคชันองค์กรหลายตัว

### [#](#prism-central) Prism Central

Prism Central เป็น multi-cloud control และ management interface สำหรับ Nutanix Prism Central สามารถจัดการคลัสเตอร์ Nutanix หลายตัวและทำหน้าที่เป็นจุดรวมสำหรับการตรวจสอบและ analytics

### [#](#node) Node

เซิร์ฟเวอร์ x86 มาตรฐานอุตสาหกรรมพร้อม SSD ที่ติดกับเซิร์ฟเวอร์และ HDD ตามต้องการ (All-Flash & Hybrid Options)

### [#](#block) Block

2U rackmount chassis บรรจุ 1, 2, หรือ 4 โหนดพร้อมแหล่งจ่ายไฟและพัดลมร่วมกันและไม่มี backplane ร่วมกัน

### [#](#storage-pool) Storage Pool

Storage pool คือกลุ่มของอุปกรณ์จัดเก็บข้อมูลฟิสิคัล รวมถึงอุปกรณ์ PCIe SSD, SSD, และ HDD สำหรับคลัสเตอร์

### [#](#storage-container) Storage Container

Container คือส่วนย่อยของพื้นที่จัดเก็บข้อมูลที่มีอยู่ที่ใช้เพื่อนำ storage policy ไปใช้

### [#](#anatomy-of-a-read-i-o) Anatomy of a Read I/O

Performance and Availability

-   อ่านข้อมูลในพื้นที่
-   เข้าถึงจากระยะไกลเฉพาะเมื่อข้อมูลไม่มีอยู่ในพื้นที่

### [#](#anatomy-of-a-write-i-o) Anatomy of a Write I/O

Performance and Availability

-   เขียนข้อมูลในพื้นที่
-   จำลองบนโหนดอื่นสำหรับ high availability
-   กระจาย replica ทั่วทั้งคลัสเตอร์เพื่อประสิทธิภาพสูง

## [#](#nutanix-flow) Nutanix Flow

### [#](#application-security-policy) Application Security Policy

ใช้ application security policy เพื่อรักษาความปลอดภัยแอปพลิเคชันโดยระบุ traffic source และ destination ที่อนุญาต

### [#](#isolation-environment-policy) Isolation Environment Policy

ใช้ isolation environment policy เมื่อคุณต้องการบล็อก traffic ทั้งหมด ไม่ว่าจะเป็นทิศทางใด ระหว่างกลุ่ม VM สองกลุ่มที่ระบุโดย category VM ภายในกลุ่มสามารถสื่อสารกันได้

### [#](#quarantine-policy) Quarantine Policy

ใช้ quarantine policy เมื่อคุณต้องการแยก VM ที่ถูกบุกรุกหรือติดเชื้อและต้องการให้ forensics ตามต้องการ คุณไม่สามารถแก้ไข policy นี้ได้ และสองโหมดในการ quarantine VM คือ Strict หรือ Forensic

**Strict:** ใช้ค่านี้เมื่อคุณต้องการบล็อก traffic ขาเข้าและขาออกทั้งหมด

**Forensic:** ใช้ค่านี้เมื่อคุณต้องการบล็อก traffic ขาเข้าและขาออกทั้งหมดยกเว้น traffic ไปและจาก category ที่มีเครื่องมือ forensic

#### [#](#apptier) AppTier

เพิ่มค่าสำหรับ tier ในแอปพลิเคชันของคุณ (เช่น web, application\_logic, และ database) ให้กับ category นี้และใช้ค่าเพื่อแบ่งแอปพลิเคชันออกเป็น tier เมื่อกำหนดค่า security policy

#### [#](#apptype) AppType

เชื่อมโยง VM ในแอปพลิเคชันของคุณกับประเภทแอปพลิเคชันในตัวที่เหมาะสม เช่น Exchange และ Apache\_Spark คุณยังสามารถอัปเดต category เพื่อเพิ่มค่าสำหรับแอปพลิเคชันที่ไม่อยู่ใน category นี้

#### [#](#environment) Environment

เพิ่มค่าสำหรับ environment ที่คุณต้องการแยกออกจากกัน แล้วเชื่อมโยง VM กับค่าเหล่านั้น
