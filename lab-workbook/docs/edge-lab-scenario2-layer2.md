# Create the Subnet Extension

การสร้าง **Subnet Extension** จำเป็นต้องทำจาก **Prism Central** เพียงแห่งเดียวเท่านั้น โดยอาศัยคู่ของ **gateways** ที่สร้างไว้ก่อนหน้านี้ เราจะเริ่มสร้าง **L2 subnet extension** จาก **Prism Central** ของ **Core** cluster

**Subnet Extension** จะถูกสร้างขึ้นเพียงครั้งเดียวต่อคู่ของ cluster โปรดทำตามขั้นตอนใน [guided Subnet Extension demo](https://nutanix.storylane.io/share/85ff2hct9ydo?flow=4&scale=true) และกลับมาอ่านคำแนะนำเหล่านี้เมื่อคุณถึงขั้นตอน **Enable DR**: **5 - Enable DR**

1. เลือก **> Network and Security > Connectivity**
    
2. **เลือก** แท็บ **Subnet Extension**
    
3. คลิกปุ่ม **Create Subnet Extension**
    
4. เลือก Extend Subnet over **VTEP**
    
5. เลือก **Across Availability Zones** จากรายการ **drop-down**
    
6. กรอกข้อมูลในฟิลด์ต่างๆ ด้วยข้อมูลต่อไปนี้:
    
    - **Local Availability zone:** **Local AZ**
    - **Remote Availability Zone:** (**PC IP address of the Cloud Cluster**)
    - **Subnet Type Local and Remote:** VLAN
    - **Local Cluster:** Core
    - **Remote Cluster:** Cloud
    - **Subnet, Local and Remote:** stretch-net
    - **Local Gateway IP Address/Prefix:** _x.x.x.129/25_
    - **Remote Gateway IP Address/Prefix:** _x.x.x.254/25_
    - **Local IP Address:** x.x.x.198
    - **Remote IP Address:** x.x.x.199
    - **VxLAN Network Identifier (VNI):** 891
        - ใช้ **VLAN tag** ของ **Core** stretch-net
        - ตัวระบุนี้สามารถเป็นค่าใดก็ได้ ตราบใดที่เป็นค่าที่ไม่ซ้ำกันระหว่าง **AZs** เหล่านี้และตรงกันทั้งสองฝั่ง

    !!! note

        ฟิลด์ **Subnet** อาจปรากฏเป็นสีเทา ซึ่งบ่งบอกว่าไม่สามารถแก้ไขได้ นี่เป็นปัญหาของ **UI** แต่จริงๆ แล้วฟิลด์นี้ยังสามารถแก้ไขได้ โปรดคลิกที่รายการ **Subnet drop down** เพื่อเลือก **stretch-net**

    ![image](images/Create-Subnet-Ext-01.e2864170.png)

    !!! note

        **Local IP Address** และ **Remote IP Address** คือ **addresses** ที่ว่างอยู่ในเครือข่ายที่ทำ **stretched** ซึ่งอยู่นอกขอบเขต **DHCP** ที่กำหนดไว้ เพื่อหลีกเลี่ยงความขัดแย้งกับ **guest VMs** โดย **IP addresses** เหล่านี้จะถูกกำหนดให้กับ **network gateway VMs** ภายใน **extended subnet** ที่ชื่อ **stretch-net**

### Verify Subnet Extension

เมื่อสร้าง **subnet extension** เรียบร้อยแล้ว คุณควรเห็นข้อมูลต่อไปนี้บนแท็บ **Connectivity, Subnet Extensions** โดยสถานะ **Connection Status** ที่เป็น **Connected** หมายความว่า **local gateways** ทั้งสองฝั่งสามารถติดต่อกันได้ และควรจะสามารถแลกเปลี่ยน **traffic** จากไซต์หนึ่งไปยังอีกไซต์หนึ่งได้

![image](images/Subnet-Ext-Status.05e4321a.png)

อาจใช้เวลาสักครู่เพื่อให้การเชื่อมต่อเสร็จสมบูรณ์ โปรดสอบถามผู้สอนเพื่อขอความช่วยเหลือหากคุณไม่เห็นสถานะ **Connected** หลังจากผ่านไปสองสามนาที

## Next Steps

เมื่อกำหนดค่า **layer 2 subnet extension** เรียบร้อยแล้ว คุณก็พร้อมที่จะย้าย **applications** ข้ามไซต์ได้อย่างราบรื่น

[← Back: Deploy Remote Gateways](edge-lab-scenario2-remotegw.md) | [Home](edge-getting-started.md) | [Next: Overview →](edge-lab-scenario3-overview.md)