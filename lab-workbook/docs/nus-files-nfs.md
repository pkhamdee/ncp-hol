# Create NFS Export

## Overview

ในแบบฝึกหัดนี้ คุณจะได้สร้างและทดสอบ NFSv4 export ที่ใช้สำหรับรองรับ clustered applications, เก็บ application data เช่น logging หรือเก็บ unstructured file data อื่นๆ ที่มักจะถูกเข้าถึงโดย Linux clients

## Using NFS Exports

### Enabling NFS Protocol

การดำเนินการเพียงครั้งเดียว

การเปิดใช้งาน NFS protocol จะต้องดำเนินการเพียงครั้งเดียวต่อ Files server หนึ่งตัว ซึ่งอาจจะเสร็จสมบูรณ์แล้วใน environment ของคุณ หากเปิดใช้งาน NFS แล้ว ให้ดำเนินการต่อในส่วนของ [Creating the Export](#creating-the-export)

1.  ในแท็บเบราว์เซอร์ Nutanix Files ของคุณ ให้ไปที่ **Configuration > Authentication > Directory Services**
    
2.  คลิก **Show NFS Advanced Options**
    
3.  เอาเครื่องหมายถูกออกจาก **Enable NFSv3 by default for all exports** เนื่องจากเราจะทดสอบเฉพาะ NFS4 exports ในแล็บนี้
    
4.  คลิกที่ **Update**
    
    ![](/images/1.05824aea.png)
    

### Creating The Export

1.  เลือก **Shares** แล้วคลิกที่ **Create a New Share**
    
2.  กรอกข้อมูลในช่องต่อไปนี้ แล้วคลิก **Next**
    
    -   **Name** - `User##`-logs
    -   **Description (Optional)** - File share for system logs
    -   **Share Path (Optional)** - เว้นว่างไว้
    -   **Max Size (Optional)** - เว้นว่างไว้
    -   **Select Protocol** - NFS
    
    ![](/images/2.50752892.png)
    
3.  กรอกข้อมูลในช่องต่อไปนี้ แล้วคลิก **Next**
    
    -   เลือก **Enable Self Service Restore** snapshots เหล่านี้จะปรากฏในไดเร็กทอรี .snapshot สำหรับ NFS clients
    -   **Authentication** - System (default)
    -   **Default Access (For All Clients)** - No Access
    -   คลิกที่ **Add Exceptions**
    -   **Clients with Read-Write Access** - ตัวเลขสามชุดแรกของ cluster network ของคุณตามด้วยเครื่องหมายดอกจัน (เช่น 10.38.62.*)
    
    ![](/images/3.28d1c5bb.png)
    
    โดยค่าเริ่มต้น NFS export จะอนุญาตการเข้าถึงแบบ read และ write สำหรับ host ใดๆ ที่ทำการ mount ตัว export นั้น แต่เราสามารถจำกัดสิทธิ์ให้กับ IP หรือ IP ranges ที่ระบุได้ ดังที่เราได้ทำไว้ที่นี่
    
4.  ตรวจสอบข้อมูลในหน้า **Summary** แล้วคลิก **Create**
    

### Testing The Export

คุณจะใช้ Linux Tools VM เป็น client สำหรับ NFS Files export ของคุณ

1.  ภายใน Prism Central ให้เลือก **> Compute & Storage > VMs** ค้นหาหมายเลข IP address ที่กำหนดให้กับ `USER##`-LinuxTools VM โดยดูที่คอลัมน์ _IP Addresses_
    
2.  เปิดโปรแกรม Putty เชื่อมต่อกับ `User##`-LinuxTools VM ของคุณโดยป้อน IP address ลงในช่อง _Host Name (or IP address)_ แล้วคลิก **Open**
    
3.  เข้าสู่ระบบโดยใช้ข้อมูล (credentials) ต่อไปนี้
    
    -   **Username** - `root`
    -   **Password** - `nutanix/4u`
    
4.  รันคำสั่งต่อไปนี้ ซึ่งสามารถทำได้โดยรันทีละคำสั่งแยกกัน หรือทั้งหมดพร้อมกันก็ได้ ข้อความใดๆ ที่อยู่หลังเครื่องหมาย `#` ในแต่ละบรรทัดสามารถปล่อยไว้ได้ เนื่องจากมันจะไม่ถูกนับรวมเป็นส่วนหนึ่งของคำสั่ง
    
    ```
    # Use Vault Mirror
    sudo sed -i s/mirror.centos.org/vault.centos.org/g /etc/yum.repos.d/*.repo
    sudo sed -i s/^#.*baseurl=http/baseurl=http/g /etc/yum.repos.d/*.repo
    sudo sed -i s/^mirrorlist=http/#mirrorlist=http/g /etc/yum.repos.d/*.repo
    
    sudo su - centos #ยกระดับสิทธิ์ของเรา (Elevates our privileges)
    sudo yum install -y nfs-utils #ติดตั้ง NFSv4 client
    mkdir /home/centos/filesmnt #สร้างไดเร็กทอรีใหม่
    sudo mount.nfs4 BootcampFS.ntnxlab.local:/`User##`-logs /home/centos/filesmnt #ตรวจสอบให้แน่ใจว่าคุณเปลี่ยน User## ให้ตรงกับของคุณ ทำการ Mount NFS ไปยังไดเร็กทอรีที่เราเพิ่งสร้างบนเครื่อง Linux ของเรา
    df -kh #แสดงรายการไฟล์และโฟลเดอร์ภายในโฟลเดอร์นี้
    ll /home/centos/filesmnt/ #แสดงจำนวนไฟล์ภายในโฟลเดอร์นี้
    ```
    
5.  สังเกตว่า **logs** NFS share ถูก mount อยู่ใน `/home/centos/filesmnt`
    

6.  คำสั่งต่อไปนี้จะทำการเพิ่มไฟล์ขนาด 2MB จำนวน 100 ไฟล์ที่เต็มไปด้วยข้อมูลแบบสุ่ม (random data) ลงใน `/filesmnt/logs`:
    
    ```
    sudo su - centos
    mkdir /home/centos/filesmnt/host1
    for i in {1..100}; do dd if=/dev/urandom bs=8k count=256 of=/home/centos/filesmnt/host1/file$i; done
    ```
    
7.  กลับไปที่แท็บเบราว์เซอร์ **Nutanix Files**
    
8.  คลิกที่ **Shares > `User##`-logs** เพื่อตรวจสอบ performance และ usage ของ NFS export ของคุณ ข้อมูล utilization จะได้รับการอัปเดตทุกๆ สิบนาที เราได้เตรียมตัวอย่างไว้ให้เผื่อว่าคุณไม่ต้องการรอให้ข้อมูลของคุณแสดง
    
    ![](/images/4.302a5168.png)
    
9.  คลิก < ที่บริเวณมุมซ้ายบนเมื่อคุณพร้อมที่จะดำเนินการในส่วนถัดไป
