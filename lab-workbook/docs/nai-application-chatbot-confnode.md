# Configure nodes

เมื่อ node ต่างๆ อยู่บน canvas แล้ว มากำหนดค่าให้ใช้งานได้จริงกัน

คุณจะสังเกตเห็นจุด connector หนึ่งจุดหรือมากกว่าที่ขอบของแต่ละ node นั่นคือจุดที่จะเชื่อมต่อกับ node อื่น บางส่วนเป็นการเชื่อมต่อที่จำเป็นซึ่งระบุด้วยเครื่องหมายดอกจันสีแดง connector ทางซ้ายเป็น input และทางขวาเป็น output

![Node Connectors](images/node-connectors.14826155.png)

ก่อนอื่น คุณจะกำหนดค่าข้อมูล endpoint ใน **ChatOpenAI Custom** node

1.  บน **ChatOpenAI Custom** node คลิก drop down ใต้ **Connect Credential**
    
2.  คลิก **Create New**
    
3.  ตั้งชื่อ key และ copy API key ที่รวบรวมไว้ก่อนหน้า แล้วคลิก **Add**
    
    ![API Key Configuration](images/api-key-config.b94cdb63.png)
    
4.  ใต้ **Model Name** พิมพ์ชื่อ endpoint ของคุณ (เช่น `llama-endpoint##`) ณ จุดนี้ node ควรมีลักษณะคล้ายกับภาพด้านล่าง
    
    ![OpenAI Node Config](images/openai-node.6666c1de.png)
    
5.  คลิก **Additional Parameters**
    
6.  เนื่องจาก inference engine ของเรารันบน CPU ที่ไม่มีการเร่งความเร็ว มากำหนดขนาด max token ให้จำกัดไว้ ใต้ **Max Tokens** ให้ป้อน 256
    
    !!! warning    
        ตรวจสอบให้แน่ใจว่า Max Tokens ถูกตั้งค่าเป็น 256 มิฉะนั้น chatbot ของคุณอาจไม่ส่งคืนคำตอบเนื่องจากทรัพยากรที่จำกัด
    
7.  ใต้ **BasePath** ป้อน endpoint URL ของคุณ (โดยไม่มี `/chat/completions`)
    
8.  คลิกออกจาก dialog เพื่อบันทึก คุณสามารถตรวจสอบซ้ำว่า input ถูกบันทึกโดยคลิก **Additional Parameters** อีกครั้ง
    
    ![Additional Parameters](images/additional-params.5ea734be.png)

---

[← Back: Add Nodes to Chatflow](nai-application-chatbot-addnode.md) | [Home](nai-welcome.md) | [Next: Connect Nodes →](nai-application-chatbot-connnode.md)