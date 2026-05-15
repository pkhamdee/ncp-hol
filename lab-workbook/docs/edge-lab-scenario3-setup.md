# Disaster Recovery Setup

ก่อนที่จะเริ่มทำ lab นี้ มีรายการหลายอย่างที่ต้องทำให้เสร็จสมบูรณ์ก่อน นี่คือภาพรวมของงานทั้งหมด:

- **Enable Nutanix Disaster Recovery** บน **Core** และ **Cloud Prism Central instances**
    - นี่คือการดำเนินการเพียงครั้งเดียวต่อหนึ่ง **cluster**
    - หมายเลขที่นั่ง (Seats) 01, 11, 21, 31, และ 41 จะทำหน้าที่กำหนดค่า **Core cluster**
    - หมายเลขที่นั่ง (Seats) 02, 12, 22, 32, และ 42 จะทำหน้าที่กำหนดค่า **Cloud cluster**
- ติดตั้ง **Nutanix Guest Tools** ใน **guest VM** ของคุณ โดยทุกที่นั่งจะต้องดำเนินการขั้นตอนนี้
    - **Nutanix Guest Tools** จำเป็นสำหรับการคงค่า **IP (IP preservation)** ในระหว่างทำ **DR**
- กำหนดค่า **static IP Address** ให้กับ **Linux VM** ของคุณ โดยทุกที่นั่งต้องดำเนินการ
    - ใช้ **PuTTY SSH client** ใน **VDI session** ของคุณ

## Enable Nutanix Disaster Recovery

ทำตามขั้นตอนใน [guided DR enablement demo](https://nutanix.storylane.io/share/85ff2hct9ydo?flow=5&scale=true) และกลับมาอ่านคำแนะนำเหล่านี้เมื่อคุณทำตาม demo จนจบแล้ว

1. เชื่อมต่อเข้ากับ **Core Prism Central** ด้วยชื่อผู้ใช้ `admin`
    
2. ใน **Core Prism Central** ให้คลิกที่ปุ่มตั้งค่า (ไอคอนรูปฟันเฟือง) ที่มุมขวาบนของ **web console**
    
3. คลิก **Enable Disaster Recovery** ในส่วน **Setup** ที่แถบด้านซ้าย
    - กล่องโต้ตอบ **Nutanix Disaster Recovery** จะทำการตรวจสอบเบื้องต้น (**pre-checks**) หากมีการตรวจสอบใดล้มเหลว ให้แก้ไขสาเหตุนั้นแล้วคลิก **Check Again** หรือขอความช่วยเหลือจากผู้สอนหากไม่สามารถแก้ไขได้

4. คลิก **Enable** หลังจากที่การตรวจสอบทั้งหมดผ่านเรียบร้อยแล้ว


## Install NGT

1. ใน **Core Prism Central** ให้เลือก **> Compute & Storage > VMs**
    
2. คลิกช่องติ๊กถูกถัดจาก **VM** ที่ชื่อ **Linux-User##** (โดย ## คือหมายเลขที่นั่ง 1-50 ของคุณ)
    
3. เลือก **Actions** เลื่อนลงมาแล้วเลือก **Install NGT**
    
4. ป๊อปอัพ **Install NGT** จะปรากฏขึ้น ให้เลือกปุ่มตัวเลือก **Restart as soon as the install is completed** จากนั้นคลิก **Confirm & Enter Password**
    
5. ทางด้านขวาให้กรอก **VM credentials** คือ Username = `ubuntu` / Password = `nutanix/4u` แล้วคลิก **Done**
    
6. ขั้นตอนนี้จะติดตั้ง **NGT** และทำการรีบูต **VM** ให้ตรวจสอบว่าติดตั้งสำเร็จโดยดูที่ฟิลด์ **NGT Status** ในหน้ารายการ **VM** ของ **Prism Central** ซึ่งควรจะขึ้นสถานะเป็น **Latest**
    

## Configure a Static IP for your VM

1. ใน **Core Prism Central** ให้เลือก **> Compute & Storage > VMs**
    
2. เลือกช่องติ๊กถูกถัดจาก **VM** ของคุณ แล้วคลิก **> Actions > Update**
    
3. ในแท็บ **Configuration** ให้คลิก **Next**
    
4. ในแท็บ **Resources** ให้คลิกไอคอนรูปดินสอตรงเครือข่ายที่เป็น "stretch-net"
    
5. เปลี่ยน **Assignment Type** จาก **Assign Static IP** เป็น **Assign with DHCP** แล้วคลิก **Save**
    

    !!! note

        การเลือก **Assign with Static IP** ใน **Prism** นั้นคล้ายกับความหมายของ **DHCP reservation** ซึ่งจะไม่ได้เป็นการกำหนด **static IP** ลงไปภายในตัว **guest VM** จริงๆ โดยตัว **guest VM** จะยังคงใช้ **DHCP** อยู่

        เราต้องการให้ **IP** ยังคงเดิมหลังจากทำ **failover** ดังนั้นเราจึงจำเป็นต้องกำหนดค่าเป็น **static** ภายในระบบปฏิบัติการของ **guest VM**

6. เข้าสู่ **VM** ของคุณผ่าน **SSH** ด้วย Username = `ubuntu` / Password = `nutanix/4u`
    
7. คัดลอกไฟล์กำหนดค่าเครือข่ายปัจจุบันโดยใช้คำสั่งต่อไปนี้ภายใน **SSH session**
    

    ```
    sudo cp /etc/netplan/50-cloud-init.yaml /etc/netplan/50-cloud-init.yaml.copy
    ```

8. ตรวจสอบชื่อ **adapter** และ **IP Address** ที่ใช้งานอยู่ปัจจุบันโดยใช้คำสั่งด้านล่าง

    - คำสั่ง

    ```
    ip a
    ```

    - ตัวอย่างผลลัพธ์

    ```
    ubuntu@Linux-Tools-VM:~$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo

    2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> ... <---- ใช้ interface นี้
        link/ether 50:6b:8d:e6:75:ac brd ff:ff:ff:ff:ff:ff
        altname enp0s3
        inet 10.42.231.50/25 ... scope global dynamic ens3 <-- ใช้ IP นี้
    ```

9. เปิดไฟล์กำหนดค่าเครือข่ายด้วย **nano**

    ```
    sudo nano /etc/netplan/50-cloud-init.yaml
    ```

10. คัดลอกและวางตัวอย่างลงในไฟล์กำหนดค่า **cloud-init.yaml** โดยเปลี่ยน **adapter name**, **IP Address** และ **gateway** ให้ตรงกับ **VM** และข้อมูลเครือข่ายของ **cluster** ที่คุณได้รับมอบหมาย

    - รูปแบบ (template)

    ```
    network:
      version: 2
      ethernets:
        ens3: # แทนที่ด้วยชื่อ interface ของคุณ
          dhcp4: false
          addresses:
            - 10.x.y.z/25 # แทนที่ด้วย IP address ของ VM ที่คุณได้รับ
          routes:
            - to: 0.0.0.0/0
              via: 10.x.y.129 # แทนที่ด้วย stretch-net gateway ที่คุณได้รับ
    ```

    - ตัวอย่าง

    ```
    network:
      ethernets:
        ens3:
          dhcp4: false
          addresses:
            - 10.55.89.201/25
          routes:
            - to: 0.0.0.0/0
              via: 10.55.89.129
      version: 2
    ```

11. ใช้การกำหนดค่าเครือข่ายใหม่ด้วยคำสั่งต่อไปนี้ภายใน **guest VM**

    ```
    sudo netplan apply
    ```

12. ตรวจสอบว่า **VM** ยังคงสามารถเชื่อมต่อกับเครือข่ายท้องถิ่นได้โดยการ **ping** ไปยังหมายเลข **IP** ของ **local gateway**

    - รูปแบบ

    ```
    ping 10.55.XX.128 # โดยที่ XX คือหมายเลขเครือข่ายของคุณ
    ```

    - ตัวอย่าง

    ```
    ping 10.55.89.129
    ```

## Next Steps

เมื่อ **guest VM** ของเรามีหมายเลข **static IP** และสามารถ **ping** ไปยัง **local gateway** ได้แล้ว เราก็พร้อมที่จะดำเนินการกำหนดค่า **DR** และทำการ **failover** ในขั้นตอนต่อไปครับ

[← Back: Definitions](edge-lab-scenario3-def.md) | [Home](edge-getting-started.md) | [Next: Create Category & Associate VM →](edge-lab-scenario3-cat.md)