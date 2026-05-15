# Create the Remote Gateways

!!! info

    **Remote gateway** จะถูกสร้างขึ้นเพียงครั้งเดียวต่อหนึ่ง **cluster** โปรดทำตามขั้นตอนใน [guided remote gateway configuration demo.open in new window](https://nutanix.storylane.io/share/85ff2hct9ydo?flow=3&scale=true) และกลับมาอ่านคำแนะนำเหล่านี้เมื่อคุณถึงขั้นตอน **Subnet Extension**: **4 - Create Subnet Extension**

**Local gateway** แต่ละตัวต้องชี้ไปยัง **remote gateway** เพื่อจับคู่กัน

ใน **Core** cluster เราจะสร้าง **Remote Gateway** โดยใช้ **external IP address** ที่สามารถติดต่อได้ของ **Cloud** cluster local gateway

ใน **Cloud** cluster เราจะสร้าง **Remote Gateway** โดยใช้ **external IP address** ที่สามารถติดต่อได้ของ **Core** cluster local gateway

![images](images/Create-Remote-GW-01.053b6384.png)

### Create the Remote Gateway on Core

1. เชื่อมต่อเข้ากับ **Core Prism Central** ด้วยชื่อผู้ใช้ `admin`
    
2. เลือก **> Network and Security > Connectivity**
    
3. คลิก **Create Gateway > Remote** และกรอกข้อมูลต่อไปนี้:
    
    - **Name:** Remote-Cloud-Gateway
    - **Gateway Service:** VTEP
    - **Public IP Address:** y.y.y.90
        - นี่คือ **IP** ของ **Local Gateway** บน **Cloud** cluster
        - แทนที่ y.y.y ด้วย **Cloud cluster network** ของคุณ

4. คลิก **Create** เพื่อเสร็จสิ้นการสร้าง **remote gateway** ของ **Core** cluster ที่ชี้ไปยัง **Cloud**
    

### Create the Remote Gateway on Cloud

1. เชื่อมต่อเข้ากับ **Cloud Prism Central** ด้วยชื่อผู้ใช้ `admin`
    
2. เลือก **> Network and Security > Connectivity**
    
3. คลิก **Create Gateway > Remote** และกรอกข้อมูลต่อไปนี้:
    
    - **Name:** Remote-Core-Gateway
    - **Gateway Service:** VTEP
    - **Public IP Address:** y.y.y.90
        - นี่คือ **IP** ของ **Local Gateway** บน **Core** cluster
        - แทนที่ y.y.y ด้วย **Core cluster network** ของคุณ

4. คลิก **Create** เพื่อเสร็จสิ้นการสร้าง **remote gateway** ของ **Cloud** cluster ที่ชี้ไปยัง **Core**
    

### Verify Gateway Creation

เมื่อเสร็จสมบูรณ์ หน้าจอ **gateway** ควรแสดงให้เห็นว่ามี **local gateway** ปรากฏอยู่ในแต่ละ **cluster** และสถานะของ **gateways** อยู่ในสถานะ **up** นอกจากนี้แต่ละ **cluster** ควรแสดง **remote gateway** ที่สอดคล้องกับ **IP** ของ **remote cluster network gateway**

## Next Steps

เมื่อสร้าง **local** และ **remote gateways** เรียบร้อยแล้ว ตอนนี้เราสามารถดำเนินการกำหนดค่า **subnet extension** เพื่อเชื่อมต่อเครือข่ายทั้งสองเข้าด้วยกันโดยใช้ **gateway VMs**

[← Back: Deploy Local Gateways](edge-lab-scenario2-localgw.md) | [Home](edge-getting-started.md) | [Next: Extend Layer 2 Subnet →](edge-lab-scenario2-layer2.md)