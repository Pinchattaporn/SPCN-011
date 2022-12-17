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

4.Profile Setup
            
            กรอกข้อมูลต่างๆ 
                Your name ,Your Server's name ,Choose a password ,Confirm your Password
                
5.SSH Setup

            5.1 เลือก [x] Install open SSH server
            5.2 Import SSH identity เลือก from Github
            5.3 GitHub Username :ใส่ชื่อ Username Github
            5.4 เลือก [x] Allow password authentication over SSH
            5.5 พอตั้งค่าทุกอย่างเสร็จจะขึ้นหน้าต่าง Confirm SSH keys กด Yes

6.Featured Server Snaps: Done

7.Installing System

              ขั้นตอนกำลังติดตั้ง ใช้เวลานานพอสมควร
8.ทำการตั้งค่าเวลาใหม่ **Setup timezone**
               
               ทำการ cler และใช้คำสั่งต่อไปนี้
               Sudo timedatectl set-timezone Asia/Bankok : เปลี่ยน timezone ให้เป็น localtime
               Sudo apt Update;sudo aptUpgrade -y : อับเดตเวอร์ชันซอฟต์แวร์ต่างๆ

9.ติดตั้ง **agent**

                Sudo apt install qemu-guest-agent :ติดตั้งเสร็จกด check
                Sudo systemctl start qemu-guest-agent 
                Sudo systemctl status qemu-guest-agent 
                    รอรันให้เสร็จ agent จะทำงาน ตรง jps-summary จะขึ้นข้อมูลต่างๆ
                    
 10.ติดตั้ง **namo**
  
                apt install namo
                
 11.ทดสอบการใช้ **Network**
 
                ping 1.1.1.1 : เช็คNetwork เช็คcommandว่าผ่านมั้ย
                ติดตั้ง iptils-ping ติดตั้งเพื่อให้ command ผ่าน
                     apt install iputils-ping
                     ลอง ping 1.1.1.1 อีกรอบ
                ติดตั้ง dns
                     apt install  dnsutills
                     เมื่อติดตั้งเสร็จใช้คำสั่ง dig เพื่อทดสอบ
                ติดตั้ง ntpdate
                     apt install ntpdate
                     ntpdate th.pool.ntp.org : ntp ซิ้งค์ไปหา Network time
                     whereis ntpdate :ซิ้งค์เวลาเป็นรอบ(เช่น ชั่วโมง)


