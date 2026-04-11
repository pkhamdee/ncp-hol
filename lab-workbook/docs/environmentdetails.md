# Environment Details

Nutanix Bootcamp ได้รับการออกแบบให้รันภายในสภาพแวดล้อม Nutanix Hosted POC คลัสเตอร์ Nutanix ได้รับการ provision พร้อม image, network และ VM ที่จำเป็นทั้งหมดเพื่อทำแบบฝึกหัดได้อย่างเสร็จสมบูรณ์

## Credentials

ผู้สอน Bootcamp หรือตัวแทน Nutanix จะให้ชื่อผู้ใช้แก่คุณ และหากไม่ระบุเป็นอย่างอื่น ทั้งหมดจะใช้ `HPOC-PASSWORD` เดียวกันเพื่อเข้าถึงทรัพยากร

ชื่อผู้ใช้แบ่งออกเป็นสองหมวดหมู่หลัก: **HPOC Lab Access** และ **Cluster Level Access**

!!! note
    `HPOC-PASSWORD` นั้นไม่ซ้ำกันสำหรับแต่ละคลัสเตอร์ และจะถูกกำหนดโดยผู้สอน

### HPOC Lab Access Credentials

ข้อมูล Lab Access credentials มีรูปแบบที่คล้ายกันและตั้งชื่อตามคลัสเตอร์ฟิสิคัลที่คุณจะเข้าถึง ตัวอย่างเช่น คลัสเตอร์ที่อยู่ใน data center Phoenix ของเราจะมีชื่อผู้ใช้เช่น Based Clusters: `PHX-POCxxx-User01` ถึง `PHX-POCxxx-User20`

### Cluster Access Credentials

บริการต่างๆ ที่รันบนคลัสเตอร์ Nutanix อาจต้องการชื่อผู้ใช้ที่แตกต่างกันแต่จะใช้ `HPOC-PASSWORD` เดียวกัน

|Credential       |Username |Password        |
|-----------------|---------|----------------|    
|Prism Element    |admin    |`HPOC-PASSWORD` |
|Prism Central    |admin    |`HPOC-PASSWORD` |
|Controller VM    |nutanix  |`HPOC-PASSWORD` |
|Prism Central VM |nutanix  |`HPOC-PASSWORD` |

แต่ละคลัสเตอร์มี domain controller VM เฉพาะ (AutoAD) ซึ่งรับผิดชอบในการให้บริการ AD สำหรับ domain ntnxlab.local โดย domain มีผู้ใช้และกลุ่มต่อไปนี้:

|Group            |Username(s)              |Password        |
|-----------------|-------------------------|----------------|
|Administrators   |Administrator            |`HPOC-PASSWORD` | 
|SSP Admins       |adminuser01-adminuser25  |`HPOC-PASSWORD` |
|SSP Developers   |devuser01-devuser25      |`HPOC-PASSWORD` |
|SSP Consumers    |consumer01-consumer25    |`HPOC-PASSWORD` |
|SSP Operators    |operator01-operator25    |`HPOC-PASSWORD` |
|SSP Custom.      |custom01-custom25        |`HPOC-PASSWORD` |
|Bootcamp Users.  |user01-user25            |`HPOC-PASSWORD` |

## Access Instructions

สภาพแวดล้อม Nutanix Hosted POC สามารถเข้าถึงได้หลายวิธีรวมถึงการเชื่อมต่อ VPN หรือ VDI

สำหรับรายละเอียดเพิ่มเติมเกี่ยวกับการใช้สภาพแวดล้อมเหล่านี้หรือการแก้ไขปัญหา กรุณาคลิกเข้าไปที่ [HPOC Help Center](https://help.oe.nutanix.com/hpoc/en/collections/6437443-connecting-to-the-nutanix-hpoc-environment)

### Parallels VDI

-   PHX Based Clusters เข้าสู่ระบบที่: [https://phx-ras.hpoc.nutanix.com](https://phx-ras.hpoc.nutanix.com)
-   RTP Based Clusters เข้าสู่ระบบที่: [https://dm3-ras.hpoc.nutanix.com](https://dm3-ras.hpoc.nutanix.com)
-   BLR Based Clusters เข้าสู่ระบบที่: [https://blr-ras.hpoc.nutanix.com](https://blr-ras.hpoc.nutanix.com)

### Ivanti Pulse Secure VPN

#### Download the client
-   Ivanti Connect Secure - DM3: [https://dm3-vpn.xlabs.nutanix.com](https://dm3-vpn.xlabs.nutanix.com)
-   ​Ivanti Connect Secure - PHX: [https://phx-vpn.xlabs.nutanix.com](https://phx-vpn.xlabs.nutanix.com)
-   ​Ivanti Connect Secure - BLR: [https://blr-vpn.xlabs.nutanix.com](https://blr-vpn.xlabs.nutanix.com)

#### Install the client

ใน Pulse Secure Client **เพิ่ม** การเชื่อมต่อ:

PHX Example:

-   **Type** - Policy Secure (UAC) or Connection Server
-   **Name** - X-Labs - PHX
-   **Server URL** - phx-vpn.xlabs.nutanix.com

---

[← Back: Data Protection](dataprotection.md) | [Home](index.md)
