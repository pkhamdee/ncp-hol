# Migrating Shares with Move

ส่วนแบบ standalone นี้สามารถข้ามได้หากคุณต้องการโฟกัสที่ VM migration มากกว่า หรือหากคุณต้องการโฟกัสที่ Day 2 management หากคุณมีเวลา เราขอแนะนำให้ทำขั้นตอนเหล่านี้ให้เสร็จสิ้น

## File Share Migration Overview

Nutanix Files และ Nutanix Objects ช่วย customers ในการ consolidate ตัว infrastructure ของพวกเขาและลดความซับซ้อน (complexity) ในส่วนนี้เราจะอธิบายวิธีการ migrate ตัว shares หรือ exports จาก non-Nutanix file server ไปยัง Nutanix Files ด้านล่างนี้คือตัวอย่างวิธีการต่างๆ ในการ migration สำหรับ shares

1.  Native Client Tools

    -   Robocopy (SMB): Robocopy เป็น command-line tool สำหรับ Windows ที่อนุญาตให้ users สามารถ copy ตัว files, directories และแม้กระทั่ง entire drives จาก location หนึ่งไปยังอีกที่หนึ่งได้ มันถูกออกแบบมาให้มีความเสถียรและมีประสิทธิภาพ (robust and efficient) และสามารถจัดการกับ data ปริมาณมากได้อย่างรวดเร็ว
    -   Rsync (NFS): rsync เป็น command-line utility สำหรับ Unix-like operating systems ที่ใช้ในการ synchronize ตัว files และ directories ระหว่างสอง locations มันสามารถใช้เพื่อ copy ตัว files จาก location หนึ่งไปยังอีกที่หนึ่ง เพื่อสร้าง backups และเพื่อ synchronize ตัว data ระหว่าง devices

2.  Third Party Tools

    -   Peer Software (SMB)
    -   Atempo Miria (SMB and NFS)

3.  Native Nutanix Tool
    
    ด้วย Files 4.2 ตอนนี้ Nutanix ได้จัดเตรียม native SMB migration tool ที่อนุญาตให้ทำ direct migration จาก source share ไปยัง Nutanix Files target share ได้โดยตรง
    
4.  Nutanix Move
    
    แม้ว่า Files จะสามารถ migrate ตัว SMB shares ได้แบบ natively แต่มันต้องทำผ่าน command line ด้วย Move 5.0 การ migration ตัว shares ตอนนี้สามารถทำได้อย่างง่ายดายโดยตรงผ่าน Move มาดูกันว่าทำได้อย่างไร
    

source สำหรับการ migration นี้คือ Windows server ที่รัน FileShare service และมี share หนึ่งตัว share นี้มี seed data บางส่วนอยู่ภายใน destination share คือ share ที่คุณได้สร้างไว้ในส่วนก่อนหน้านี้
