# Isolation Policies

Isolation policies จะแยกกลุ่มของ entities ออกจากการสื่อสารระหว่างกัน ตัว isolation เป็น blocking policy ที่ป้องกันไม่ให้ VMs ใน category หนึ่งสื่อสารกับ VMs ใน categories อื่นๆ ในกรณีนี้ เรากำลังใช้ Environment

ด้วย Flow Network Security Next-Gen ตอนนี้คุณสามารถกำหนด single isolation policy ที่สามารถ isolates กลุ่มหรือ categories ที่แตกต่างกันได้สูงสุดถึง 32 กลุ่มออกจากกัน

requirement ของเราคือต้องแน่ใจว่าไม่มี production environment VMs ใดๆ สามารถสื่อสารกับ development environment VMs หรือ staging environment VMs ได้

![Isolation Requirement](/images/prod-dev-reqs.f5f9fbfd.png)

สิ่งนี้หมายถึงการแยก virtual machines ของคุณโดยใช้ categories **Environment: User`##`Prod**, **Environment: User`##`Dev** และ **Environment: User`##`Stage**

## Create the Isolation Policy

1.  ไปที่ **Infrastructure** > **Network and Security** > **Security Policies**

![Create Security Policy](/images/iso2.af345363.png)

2.  เลือก **\+ Create Security Policy**
    
3.  พิมพ์ **User`##`\-Isolate** เป็น policy name แทนที่ `##` ด้วยหมายเลข cluster user ของคุณ ตั้งแต่ 01 ถึง 10
    
4.  เลือกปุ่มตัวเลือก (radio button) **Isolation : Isolate Environments**
    

![Create Security Policy](/images/iso3.d0cf9e5a.png)

5.  ปล่อย Advanced Configuration เช่น IPv6 และ Policy Hit Logs ไว้ที่ default values
    
6.  เลือก **Next**
    

## Select the Isolated Categories

1.  ปล่อย **Scope** ไว้ที่ default value เป็น Global ตัว Scope จะเป็นตัวกำหนดว่า policy จะ applies กับ VMs ทั้งหมด, เฉพาะ VMs ใน VLANs หรือเฉพาะ VMs ใน specific VPCs
    
    -   ปัจจุบัน Scope ไม่สามารถเปลี่ยนแปลงได้หลังจากที่สร้าง policy แล้ว ดังนั้นควรพิจารณาการตั้งค่านี้ให้ดี การใช้ Global นั้นเหมาะสมสำหรับจุดประสงค์ของ lab นี้

โดย default แล้ว isolation policy จะถูกตั้งค่าให้ isolate 2 environments ซึ่ง requirement ของเราคือการ isolate 3 environments ออกจากกัน ดังนั้นเราต้องเพิ่ม entity

2.  เลือก **\+ Add Entity** ที่บริเวณตรงกลางด้านขวา

![Create Security Policy](/images/iso4.b5bee9c2.png)

3.  ใน drop-down แรกของ Isolated Entity VM ให้เลือก **Environment: User`##`Prod**
    
    -   คุณสามารถเริ่มพิมพ์ **User`##`P** เพื่อให้ drop down แสดงชื่อที่ตรงกันแบบเต็ม
4.  ใน drop-down ที่สองของ Isolated Entity VM ให้เลือก **Environment: User`##`Dev**
    
5.  ใน drop-down ที่สามของ Isolate Entity VM ให้เลือก **Environment: User`##`Stage**
    
    -   จำนวนของ VMs ที่ถูก protected ควรจะปรับแบบ dynamically เนื่องจากมี VMs ที่มี categories เหล่านี้อยู่แล้ว

![Create Security Policy](/images/iso5.f15b5fa9.png)

6.  คลิก **Next**
    
    -   เรามีตัวเลือกในการ Save ตัว policy นี้ โดยที่มันจะยังไม่มีผลบังคับใช้ (takes no effect) สิ่งนี้มีประโยชน์หากเราต้องการ save ในระหว่างที่เรากำลัง editing อยู่ หรือเพื่อใช้สำหรับการ cloning ในภายหลัง
    -   เรายังมีตัวเลือก **Apply (Monitor)** ซึ่งมีประโยชน์ในการเริ่มสร้าง policy โดยไม่ blocking ตัว traffic สิ่งที่เกิด Violations จะถูก visualized แต่จะไม่ถูก dropped
    -   สุดท้าย ตัวเลือก Apply (Enforce) จะทำการ blocks packets ในทันที
7.  เลือก **Apply (Monitor)**
    

![Apply and Monitor](/images/iso6.fa2aec9a.png)

8.  เลือก **Confirm**

## Verify Policy Operation

ตอนนี้เราจะมาทดสอบ Isolation policy ของเรา

1.  ใน Prism Central ไปที่ **Infrastructure** > **VMs** และทำการ filter เพื่อดู user VMs ของคุณ จดบันทึก IP address ของ **user`##`\-developer** VM ของคุณ คุณจะต้องใช้สิ่งนี้ในไม่ช้า

![Create Security Policy](/images/iso7.61ea8a96.png)

2.  คลิกขวาที่ชื่อ **user`##`\-enduser** VM ของคุณและเลือก **Launch Console**

![Create Security Policy](/images/iso8.03f52dd2.png)

3.  Log into เข้า VM โดยใช้ username nutanix และ `<PC password provided>`

![Create Security Policy](/images/iso9.a676bf75.png)

4.  จากภายใน VM console ให้ launch ตัว **Terminal Session** โดยการคลิกขวาที่ Desktop แล้วเลือก Open in Terminal หรือคลิกที่ไอคอน $\_ Terminal ที่ panel ด้านซ้ายล่าง

![Create Security Policy](/images/iso10.e77a607a.png)

5.  จากหน้าต่าง terminal ให้ ping ไปยัง IP address ของ **user`##`\-developer** VM ของคุณที่จดไว้ก่อนหน้านี้ในขั้นตอนที่ 1

```
ping 'user##-developer-vm-ip'
```

![Create Security Policy](/images/iso11.d36ef309.png)

เนื่องจาก Isolation Policy ของเราถูก applied ไว้ใน **Monitor Mode** การทำ pings เหล่านี้ควรจะสำเร็จ

ปล่อยให้การ ping นี้รันต่อไป

### View Discovered Traffic

1.  ไปที่ **Infrastructure** > **Network and Security** > **Security Policies**

Note:

Security policies จะถูกจัดกลุ่มตาม Policy Type เนื่องจาก policy ของเราคือ `Isolation` เราจึงต้องเลือก type นี้

2.  เลือก **Policy Type: Isolation**
    
3.  เลือก isolation policy ของคุณ **User`##`\-Isolate**
    

![Create Security Policy](/images/iso12.be50dd70.png)

4.  ในขณะที่ policy อยู่ใน Monitor Mode คุณจะเห็น discovered traffic

![Create Security Policy](/images/iso13.b4db26a4.png)

5.  **คลิก** ที่ discovered traffic เพื่อดูรายละเอียด

![Create Security Policy](/images/iso14.35e2b1dd.png)

อย่างที่เราเห็น นี่คือ ping จาก **user`##`enduser** VM ของเรา ซึ่งอยู่ใน **Environment: User`##`Prod** ไปยัง **user`##`\-developer** VM ของเราที่อยู่ใน **Environment: User`##`Dev**

ในขณะที่ Isolation policy อยู่ใน **Monitor Mode** traffic นี้จะถูก allowed

## Enforce the Policy

ตอนนี้เรามาเปลี่ยน Isolation Policy ของเราจาก **Monitor Mode** เป็น **Enforce Mode** กันเถอะ

1.  ที่มุมขวาบนของ Isolation Policy ให้คลิกที่ drop down สำหรับ **Apply**
    
2.  ใน drop down menu ให้เลือก **Apply (Enforce)**
    

![Create Security Policy](/images/iso15.02d06b84.png)

3.  ใน secondary confirmation box ให้พิมพ์ **ENFORCE** และเลือก **Confirm**

![Create Security Policy](/images/iso16.c9327ee5.png)

### View Blocked Traffic

ตัว traffic ที่เคยถูก discovered ไว้ก่อนหน้านี้ ตอนนี้ถูก blocked โดย Isolation Policy แล้ว สิ่งนี้เป็นเพราะ **user`##`\-enduser** VM อยู่ใน **Environment: User`##`Prod** ในขณะที่ **user`##`\-developer** VM ของเราอยู่ใน **Environment: User`##`Dev**

![Create Security Policy](/images/iso17.158a0d85.png)

1.  นำทางกลับไปยัง console session ของ **user`##`\-enduser** VM ของคุณ
    
2.  การทำ pings ไปยัง **user`##`\-developer** VM ยังสำเร็จอยู่หรือไม่?
    

![Create Security Policy](/images/iso18.77c1c880.png)

3.  หากทุกอย่างทำอย่างถูกต้อง มันไม่ควรจะสำเร็จแล้ว!

## Takeaways

![Create Security Policy](/images/prod-dev-reqs.f5f9fbfd.png)

Isolation policies เป็นเครื่องมือสำคัญ (key tool) ในการแยกกลุ่มของ VMs ไม่ว่า VMs เหล่านั้นจะอยู่ใน VLAN เดียวกัน หรือแม้แต่อยู่บน host เดียวกันหรือไม่ก็ตาม policies เหล่านี้สามารถถูก apply แบบ globally ข้าม VLANs และ VPCs, จะเจาะจง (specific) เฉพาะ VLANs, หรือเจาะจงเฉพาะ VPCs ก็ได้
