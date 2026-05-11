# Glossary

## Nutanix Core

### AOS

AOS ย่อมาจาก Acropolis Operating System และเป็น OS ที่รันบน Controller VMs (CVMs)

### Pulse

Pulse ให้ข้อมูลการวินิจฉัยระบบ (diagnostic system data) แก่ทีมสนับสนุนลูกค้าของ Nutanix เพื่อให้พวกเขาสามารถให้การสนับสนุนเชิงรุกและรับรู้บริบทสำหรับ Nutanix solutions ได้

### Prism Element

Prism Element คือ native management plane สำหรับ Nutanix เนื่องจากการออกแบบนั้นอิงจาก consumer product interfaces จึงใช้งานได้ง่ายและเข้าใจได้เป็นธรรมชาติมากกว่า enterprise application interfaces หลายๆ ตัว

### Prism Central

Prism Central คืออินเทอร์เฟซสำหรับการควบคุมและจัดการแบบ multi-cloud สำหรับ Nutanix โดย Prism Central สามารถจัดการ Nutanix clusters หลายคลัสเตอร์และทำหน้าที่เป็นจุดรวบรวมสำหรับการติดตามและการวิเคราะห์ (monitoring and analytics)

### Node

x86 server มาตรฐานอุตสาหกรรมที่มี SSD ที่ติดมากับเซิร์ฟเวอร์และ HDD เสริม (ตัวเลือก All-Flash & Hybrid)

### Block

ตัวถัง (chassis) แบบ 2U rackmount ประกอบด้วย 1, 2, หรือ 4 nodes ที่ใช้พลังงานและพัดลมร่วมกัน โดยไม่มีแผงวงจรด้านหลัง (shared backplane) ร่วมกัน

### Storage Pool

Storage pool คือกลุ่มของอุปกรณ์จัดเก็บข้อมูลทางกายภาพ รวมถึงอุปกรณ์ PCIe SSD, SSD, และ HDD สำหรับ cluster

### Storage Container

Container คือส่วนย่อยของ storage ที่มีอยู่ซึ่งถูกนำมาใช้เพื่อปรับใช้ (implement) storage policies

### Anatomy of a Read I/O

Performance and Availability

-   ข้อมูลถูกอ่านแบบ locally
-   การเข้าถึงจากระยะไกล (Remote access) จะเกิดขึ้นก็ต่อเมื่อไม่มีข้อมูลอยู่ในระดับ local

### Anatomy of a Write I/O

Performance and Availability

-   ข้อมูลถูกเขียนแบบ locally
-   มีการทำ Replicate ไปยัง nodes อื่นๆ เพื่อความพร้อมใช้งานสูง (high availability)
-   Replicas จะกระจายอยู่ทั่วคลัสเตอร์เพื่อประสิทธิภาพสูง (high performance)

## Nutanix Flow

### Application Security Policy

ใช้ application security policy เพื่อรักษาความปลอดภัยให้กับแอปพลิเคชันโดยระบุแหล่งที่มาและปลายทางของ traffic ที่อนุญาต

### Isolation Environment Policy

ใช้ isolation environment policy เมื่อคุณต้องการบล็อก traffic ทั้งหมด ไม่ว่าจะทิศทางใดก็ตาม ระหว่างสองกลุ่มของ VMs ที่ถูกระบุโดย category ของพวกมัน ซึ่ง VMs ภายในกลุ่มยังคงสามารถสื่อสารกันได้

### Quarantine Policy

ใช้ quarantine policy เมื่อคุณต้องการแยก (isolate) VM ที่ถูกเจาะระบบหรือติดไวรัส และอาจต้องการนำไปตรวจสอบทางนิติวิทยาศาสตร์ (forensics) คุณไม่สามารถแก้ไข policy นี้ได้ และสองโหมดในการ quarantine VM คือ Strict หรือ Forensic

Strict: ใช้ค่านี้เมื่อคุณต้องการบล็อก inbound และ outbound traffic ทั้งหมด

Forensic: ใช้ค่านี้เมื่อคุณต้องการบล็อก inbound และ outbound traffic ทั้งหมดยกเว้น traffic ที่เข้าและออกจาก categories ที่มี forensic tools

#### AppTier

เพิ่มค่าสำหรับ tiers ในแอปพลิเคชันของคุณ (เช่น web, application_logic, และ database) ลงใน category นี้และใช้ค่าเหล่านั้นเพื่อแบ่งแอปพลิเคชันออกเป็น tiers เมื่อทำการกำหนดค่า security policy

#### AppType

เชื่อมโยง VMs ในแอปพลิเคชันของคุณเข้ากับ built-in application type ที่เหมาะสม เช่น Exchange และ Apache_Spark คุณยังสามารถอัปเดต category เพื่อเพิ่มค่าสำหรับแอปพลิเคชันที่ไม่ได้อยู่ในรายการใน category นี้ได้

#### Environment

เพิ่มค่าสำหรับ environments ที่คุณต้องการแยกออกจากกัน จากนั้นเชื่อมโยง VMs เข้ากับค่าเหล่านั้น
