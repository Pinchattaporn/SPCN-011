# SPCN-011
# เขียนอธิบายขั้นตอนการใช้งานของ 3 หัวข้อข้างต้น พร้อมรูปประกอบของชิ้นงานตนเอง
**ขั้นตอนสร้าง vm and ct on Proxmox cluster**

**create master vm (ubuntu-22.04)**

1.เลือก Creat Virtual Machine

* 1.1 ตั้งค่า General

          1.1 ตั้งชื่อ Name โดยใช้อักษร 3ตัวตามชื่อจริง+กับVm ID 
            
          1.2 Resource pool=SpecialTopics
          
* 1.2 ตั้งค่า OS

          iso=Ubuntu-22.04.1-live-server-amd64.130
          
* 1.3 System=ตามค่า default

* 1.4 Disk=ตามค่า default

          Storage มี 2 พื้นที่คือ Cept-std กับ nas9 เลือกตามที่เราต้องการได้เลย
          
* 1.5  CPU=ตามค่า default

          Sockets=1 , cores=1
          
* 1.6 Memory=2048

* 1.7 Netwoek=ตามค่า default

* 1.8 Confirm

            กดเลือก Start after created 
            กดเลือก Finish 

2.พอตั้งค่าทุกอย่างเสร็จสามารถดูภาพรวมได้ที่ Summary

3.Setup Console

* 3.1 language=English

* 3.2 Installer update available

             เลือก Continue without updating :ติดตั้งแบบยังไม่ต้อง update เพื่อความรวดเร็ว
             
* 3.3  Keyboard configuration:เลือก Done

* 3.4  Choose Type of install

             เลือก [x] Ubuntu Server(minimized) และกดเลือก Finish

* 3.5 Network Connectious : เลือก Done

* 3.6 Configure Proxy : เลือก Done

* 3.7 Configure  Ubuntu archive mirror : เลือก Done

* 3.8 Cuided Storage configuration
  
             เลือกตามค่า default และกดเลือก Done
             
* 3.9 Storage configuration : เลือก Done

* 3.10 เมื่อ setup ทุกอย่างเสร็จ จะขึ้นหน้าต่าง Confirm destructive action

            เลือก Continue เพื่อทำการ Setup Profile
