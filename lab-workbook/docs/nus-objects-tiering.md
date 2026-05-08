# Tiering

## Overview

Tiering ช่วยลดค่าใช้จ่ายและรองรับ hybrid cloud strategy สามารถ configure tiering ใน Nutanix Objects ได้ เพื่อให้เมื่อ objects มีอายุถึงเกณฑ์ที่กำหนด พวกมันจะถูกย้ายไปยังอีก location หรือ "endpoint" โดยอัตโนมัติและไร้รอยต่อ (transparently) endpoint สามารถอยู่บน public cloud (AWS, Azure, GCP) หรือ on-premises object store อื่นๆ ได้ ตราบใดที่มันเป็น S3 compliant อย่างเคร่งครัด

## Possible Tiering Configurations

Nutanix Objects สามารถทำ tiering ไปยัง S3-compatible objects store provider ใดๆ ก็ได้

## Requirements

คุณจำเป็นต้องมี AWS account (ที่มีอยู่แล้วหรือสร้างใหม่) และสิ่งต่อไปนี้เพื่อทำ tiering ให้สำเร็จ

-   **Source S3 Storage**
    
    -   Source S3 access URL
    -   Source Access key
    -   Source Secret key
-   **Destination S3 Storage**
    
    -   Destination S3 access URL
    -   Destination Access key
    -   Destination Secret key
-   **Networking between source and destination**
    
    -   Physical connectivity
    -   Firewall และ security ที่อนุญาต connection นี้

แผนภาพด้านล่างแสดงอีกหนึ่ง use case สำหรับ Objects tiering โดย high-performance all-flash Nutanix Objects cluster จะถูกแทรกอยู่ระหว่าง application และ low-performance, high-capacity object store ประสิทธิภาพของ Application จะถูกเร่งความเร็วอย่างมากเนื่องจาก app กำลังทำการ I/O กับ high-performance storage อย่างไรก็ตาม ด้วย backend tiering ในท้ายที่สุด data จะถูกเก็บลงใน high-capacity object store ซึ่งสามารถจัดเก็บได้อย่างคุ้มค่า (cost-effectively)

![](/images/tieringdesign2.16f93840.png)

## Lab Setup

ใน lab นี้ คุณจะได้ set up tiering จาก local Nutanix Objects bucket ไปยัง bucket ใน AWS S3

![](/images/tieringdesign3.4c1cedf6.png)

ในระดับภาพรวม (high level) เราจะ implement สิ่งต่อไปนี้:

-   Create an AWS bucket เพื่อเป็น tiering destination
-   Setup Endpoint ใน Object Store configuration
-   Setup Lifecycle policies ภายใน bucket configuration

### Creating AWS S3 Bucket As A Tiering Destination

ในส่วนนี้ คุณจะได้ configure AWS S3 bucket, set up access permissions และรับ access และ secret keys

คุณสามารถ set up AWS account ได้อย่างรวดเร็ว หรือใช้ account ที่มีอยู่ (ถ้ามี) และทำ tasks ต่อไปนี้โดยใช้ AWS Free Tier program เพียงแต่ต้องระมัดระวังเพื่อให้แน่ใจว่าจำนวน data ที่ถูกเขียนจะยังคงอยู่ต่ำกว่า 50 MB

#### Create an AWS S3 Bucket

1.  Sign in เข้าสู่ AWS Management Console และเปิด Amazon S3 console ที่ [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/)
    
2.  เลือก **Create bucket**
    
3.  ใน Bucket name ให้ป้อนชื่อย่อของคุณ (ในที่นี้ **lnb** เป็นตัวอย่าง) **lnb-bucket** ซึ่งเป็น DNS-compliant name สำหรับ bucket name ของคุณ
    
    ![](/images/tiering1.53c06b6f.png)
    
4.  เลือก region ที่คุณต้องการ (ส่วนใหญ่จะถูกเลือกโดยอัตโนมัติ) ให้เลือก region ที่ใกล้ที่สุดสำหรับ tiering
    
5.  เลือก **Block all public access**
    
6.  เลือก **Disable** Bucket versioning
    
7.  เลือก **Disable** Server-side encryption
    
8.  คลิกปุ่ม **Create Bucket** ที่ด้านล่างของหน้า ตอนนี้คุณจะเห็น bucket ของคุณใน list
    
    ![](/images/tiering2.e8df0ac3.png)
    

#### Setup Access For An AWS S3 Bucket

1.  ไปที่ AWS IAM Management Console ของคุณ [ที่นี่](https://console.aws.amazon.com/iamv2)
    
2.  เลือก **Users > Add User**
    
3.  ป้อน **lnb-bucket-user** เป็น user name
    
4.  เลือก **Access key - Programmatic access**
    
5.  ในหน้าต่างถัดไป เลือก **Add user to group**
    
6.  เนื่องจากเรายังไม่มี group ให้คลิกที่ **Create group**
    
    ![](/images/tiering3.faf915bb.png)
    
7.  ป้อน **s3access** เป็น user group name
    
8.  ในช่อง input ของ **Filter Policies** ให้พิมพ์ **s3** และเลือก **AmazonS3FullAccess** เป็น policy ซึ่งจะให้สิทธิ์ (permissions) ทั้งหมด คุณสามารถสำรวจ permission policies อื่นๆ เพิ่มเติมได้ตามต้องการ
    
    ![](/images/tiering4.9773595f.png)
    
9.  คลิกที่ **Create group**
    
10.  เลือก **s3access** เป็น user group name และคลิกที่ **Next: Tags** ที่ด้านล่างของหน้าจอ
    
    ![](/images/tiering5.18e70ed8.png)
    
11.  คลิกที่ **Next: Review**
    
12.  คลิกที่ **Create user**
    
13.  ตอนนี้คุณจะเห็น success message ตามด้วย download options สำหรับ access และ secret key
    
14.  Download ไฟล์ CSV ของ access และ secret key
    
    !!! note
        ตรวจสอบให้แน่ใจว่าได้ download ไฟล์ CSV นี้และเก็บไว้อย่างปลอดภัย เนื่องจากจะสามารถทำขั้นตอนนี้ได้เพียงครั้งเดียวเท่านั้น
    
    ![](/images/tiering6.54c4a74c.png)
    
15.  คลิกที่ **Close**
    

คุณได้ set up access ไปยัง AWS S3 bucket ของคุณสำเร็จแล้ว

### Setup Endpoint In Object Store Configuration

#### Configure Endpoint

1.  Login เข้าสู่ Prism Central instance ของคุณ
    
2.  นำทางไปที่ **> Services > Objects**
    
3.  เลือก Objects store ของคุณ
    
    ![](/images/tiering7.6efa0f87.png)
    
4.  นี่จะเปิด browser tab ใหม่พร้อมกับ additional settings สำหรับ objects store ที่คุณเลือก
    
5.  เลือก **Tiering Endpoint** และคลิกที่ **Create**
    
    หากคุณเป็นคนแรกที่สร้าง tiering endpoint ให้คลิกที่ **+Add**
    
    ![](/images/tiering8.e0b52641.png)
    
6.  ใน add endpoint wizard ให้ป้อนรายละเอียดต่อไปนี้
    
    -   Name of the Endpoint - **AWS Tiering Endpoint** (ตั้งชื่อที่ระบุได้ง่าย)
    -   Endpoint Type - **Nutanix Objects**
    -   Service Host - **s3.ap-southeast-2.amazonaws.com** (ค่านี้จะเปลี่ยนไปขึ้นอยู่กับ AWS region ของคุณ)
    -   Bucket Name - **lnb-bucket** (นี่คือชื่อของ bucket ที่คุณสร้างในส่วนก่อนหน้าใน AWS)
    -   Access Key - **access key จาก CSV ที่คุณ download** ในส่วนก่อนหน้า
    -   Secret Key - **secret key จาก CSV ที่คุณ download** ในส่วนก่อนหน้า
    -   Skip SSL certificate validation - **Checked**
    
    ![](/images/tiering9.1c282ae1.png)
    
7.  คลิกที่ **Save**
    
8.  ตอนนี้คุณจะสามารถเห็น endpoint ใน Object Store configuration ของคุณ
    
    ![](/images/tiering10.ebc76fe5.png)
    

คุณได้ set up tiering endpoint ที่อยู่บน AWS สำเร็จแล้ว

#### Configure Lifecycle Policies

Lifecycle policies อนุญาตให้ tiering ถูก scheduled จาก source bucket ไปยัง target bucket ได้โดยไม่ต้องคำนึงถึง location

ในส่วนนี้ เราจะสร้าง lifecycle policy เพื่อ tier data จาก bucket ของ Nutanix Object ที่คุณสร้างใน [Objects Versioning Access Control](/unified-storage/objects_versioning_access_control/objects_versioning_access_control.html) ไปยัง AWS bucket ที่คุณสร้างไว้ก่อนหน้านี้

1.  ภายใน Prism Central ให้นำทางไปที่ **>Services > Objects**
    
2.  เลือก Objects store ของคุณ
    
3.  คลิกที่ source bucket ของคุณ _your-name_\-**my-bucket** (อันที่คุณสร้างที่นี่ [here](/unified-storage/objects_buckets_users_access_control/objects_buckets_users_access_control.html))
    
    ![](/images/tiering11.db354b34.png)
    
4.  คลิกที่ **Lifecycle** และคลิกที่ **Create Rule**
    
    ![](/images/tiering12.f6b1be64.png)
    
5.  ป้อนชื่อที่มีความหมายและระบุได้ง่าย ตัวอย่างเช่น **tier-to-aws-ap-southeast-2.amazonaws.com** ซึ่งเป็นการระบุ region ของ tiered data
    
6.  เลือก **All Objects**
    
    !!! note
        คุณยังสามารถใช้ tags เป็น option ในการเลือก objects เพื่อทำ tier ได้
    
    ![](/images/tiering13.c532d1fd.png)
    
7.  คลิกที่ **Next**
    
8.  เลือก **AWS Tiering Endpoint**
    
9.  ตั้งค่า tiering เป็น **1** วัน (days) หลังจาก objects creation date ใน source bucket
    
10.  คุณยังสามารถเลือก expiration เป็น **2** วัน (days) ใน destination storage เป็นตัวอย่าง เพื่อให้แน่ใจว่าคุณจะไม่เจอ massive bill ใน public cloud สำหรับวัตถุประสงค์ในการ testing
    
11.  คลิกที่ **Add Action** และเลือก expire Action อื่น
    
12.  เลือก **Multipart Uploads** และ **2** วัน (days) หลังจาก last creation date บน destination bucket
    
    ![](/images/tiering14.790a34da.png)
    
13.  คลิกที่ **Next**
    
14.  ทบทวน (Review) configuration ของคุณ และคลิกที่ **Done**
    
    ![](/images/tiering15.b987ae4e.png)
    

#### Verify Tiering

ส่วนนี้จะทำการตรวจสอบ (verify) tiering status ทั้งในฝั่ง source และ destination

1.  เนื่องจาก source bucket ของคุณถูก populated ด้วย data แล้ว tiering จะเริ่มต้นหลังจากหนึ่งวัน
    
    !!! note
        หากคุณกำลังทำเฉพาะ lab ของ Objects Tiering:
    
    -   สร้าง source bucket ของคุณโดยใช้ procedure ในส่วน _Create Bucket In Prism_ ใน [this section](/unified-storage/objects_versioning_access_control/objects_versioning_access_control.html)
    -   Populate source bucket ของคุณด้วย objects (data) โดยใช้ procedure _Uploading Multiple Files to Buckets with Python)_ ใน [Objects CLI Scripts](/unified-storage/objects_cli_scripts/objects_cli_scripts.html)
    
2.  เมื่อ tiering สำเร็จแล้ว คุณจะเห็น Tiering status บน source bucket ของคุณ **your-name-my-bucket > Summary**
    
    ![](/images/tiering16.a4f06c7a.png)
    
3.  บน destination AWS **lnb-bucket** คุณจะเห็น data ดังต่อไปนี้: หมายเหตุว่านี่อาจแตกต่างออกไปสำหรับ bucket ของคุณ
    
    ![](/images/tiering17.c20a9e52.png)
    

คุณได้ทำการ tiering จาก Nutanix Objects ไปยัง AWS S3 สำเร็จแล้ว

## Takeaways

สิ่งสำคัญที่คุณควรรู้เกี่ยวกับ **Nutanix Objects Tiering(Lifecycle Policies)** คืออะไร?

-   Nutanix Objects ช่วยให้ configure เพื่อ tiering data ไปยัง object stores อื่นๆ (cloud และ on-premises) ได้ง่าย
-   Tiering policies จำเป็นต้องได้รับการ configured ที่ฝั่ง source (provider) ของ bucket
    -   ตัวอย่างเช่น: การทำ Tiering จาก Nutanix Objects ไปยัง AWS จำเป็นต้องได้รับการ configured ที่ Nutanix PC (Prism Central)
    -   ตัวอย่างเช่น: การทำ Tiering จาก AWS S3 ไปยัง AWS Glacier จำเป็นต้องได้รับการ configured ที่ AWS Console
-   Nutanix ช่วยให้ applications สามารถจัดเก็บและทำ tier data ไปยัง S3-based object storage ใดๆ ได้โดยไม่มีการ lock-in
-   ฟีเจอร์ Nutanix Object tiering จะจัดกลุ่ม objects เข้าด้วยกันให้เป็น data chunk ที่มีขนาดใหญ่ขึ้น เพื่อลดค่าใช้จ่าย (save costs) ในส่วนของ PUT requests ใน S3
