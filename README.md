# SPCN-011
**เขียนอธิบายขั้นตอนการใช้งานของ 3 หัวข้อข้างต้น พร้อมรูปประกอบของชิ้นงานตนเอง**
**ขั้นตอนสร้าง vm and ct on Proxmox cluster**

# create master vm (ubuntu-22.04)

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
                    รอรันให้เสร็จ agent จะทำงาน ตรง ips-summary จะขึ้นข้อมูลต่างๆ
  ![image](https://user-images.githubusercontent.com/110905426/208231778-95ae73d3-c489-41f1-a155-c3701a945158.png)

                    
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

12.ทำการ **reboot ระบบ

                และ recheck command ที่เราติดตั้งไว้
            
13.ทำการปิดเครื่อง **เพื่อ Convert to template**

                 Convert to template ตัว Vm ล่าสุดที่เราได้ทำการ Setup ค่าต่างๆไว้
                 
![image](https://user-images.githubusercontent.com/110905426/208232006-b0b5c6ce-9924-4f30-b9f3-a8c2f693abac.png)

14.**clone from template สร้าง vm ใหม่จำนวน 2 vm**

![image](https://user-images.githubusercontent.com/110905426/208234602-2a6fc5d8-e66c-4fba-b581-968232b13cbd.png)

* 14.1 กด Start ตัวเครื่อง clone 
* 14.2 เข้า console และ reboot ระบบ

                  เข้า login ใส่ password
                  เช็ค timezone และการตั้งค่าอื่นๆ
                  
* 14.3 **Change hostname ใหม่**

                   Sudo hostnameectl set-hostname........(ชื่อที่เราจะเปลี่ยน)
                       ถ้าเปลี่ยนเสร็จชื่อใหม่ยังไม่ขึ้น ให้ใช้คำสั่ง bash
           
* 14.4 **Change ip แต่ละเครื่อง**
                    
                    เปลี่ยนค่าไอดีตัว vm เพื่อทำการขอ ip ใหม่ เพื่อไม่ให้ ip ที่เกิดจากการ โคลนจากตัว master มี ip ที่ซ้ำกัน ด้วยคำสั่ง
                                        เช็ค ip เครื่องด้วยคำสั่ง ip a
                                        sudo -i
                                        rm /var/lib/dbus/machine-id
                                        nano /etc/machine-id เพื่อลบ id เก่าออก
                                        ln -s /etc/machine-id /var/lib/dbus/machine-id ทำการ link เชื่อมต่อกับ machine-id จะได้ไม่ต้องแก้หลายที่
                                        reboot เพื่อรี ip ใหม่
 15. vm(master) และ clone from template สร้าง vm ใหม่จำนวน 2 vm (สำเร็จ)
 
 **Summary**
 
 **master**
 
 ![image](https://user-images.githubusercontent.com/110905426/208235502-48e9c8b3-7aa7-4ee6-bb85-f4caae42b217.png)

                   
![image](https://user-images.githubusercontent.com/110905426/208235254-0e762741-d6d4-47b9-a0c3-a6e7d9d8a01b.png)

**Clone 1**

![image](https://user-images.githubusercontent.com/110905426/208235345-1509143e-baf8-48c8-b17a-8395324f1c6c.png)

**Clone 2**

![image](https://user-images.githubusercontent.com/110905426/208235383-68784dc0-0bcb-44ac-8590-babdaa786a31.png)

**Watch**

**clone**

![image](https://user-images.githubusercontent.com/110905426/208235745-269107e7-7508-49ca-b9a0-7d9df69c410a.png)

**clone 2**

![image](https://user-images.githubusercontent.com/110905426/208235800-f291b79b-948f-48d6-a5aa-b656fef392fc.png)


# create vm from other os 

* สร้าง Create VM ตั้งค่าต่างๆตามที่กำหนด โดยจะใช้ระบบปฏิบัติการ Kali


summary

![image](https://user-images.githubusercontent.com/110905426/208296654-d8b5202d-91f3-40b4-8b8d-538906baa599.png)

![image](https://user-images.githubusercontent.com/110905426/208296688-7a72f405-c97f-4174-af3f-658502b3b7ec.png)





# create container template (select from CT list)

1.Create LXC Container
             
             การตั้งค่าส่วนต่างๆจะคล้าย Vm
             ตั้งค่าส่วนต่างๆตามค่า  default
                  Network ipv4 เลือก DHCP
                  Network ipv6 เลือก SLAAC
             พอตั้งค่าทุกอย่างเสร็จกด Finish

2. เข้า console ทำการเช็คค่าต่างๆ
              
              เช็ค timezone และ Setup
                    sudo timedatectl set-timezone Asia/Bangkok
                    date
              เช็ค Network
                    ping 1.1.1.1
              เช็ค package
                    apt update;  apt upgrade -y
                    รออับเตดเสร็จและ clear หน้า console

3.container ที่สร้างสำเร็จ

**Summary**

![image](https://user-images.githubusercontent.com/110905426/208236224-6e09f263-a479-48c6-b0f5-814965070312.png)

![image](https://user-images.githubusercontent.com/110905426/208236234-4eb043d1-c7cd-45fb-85aa-5a7271e1b1b2.png)


**console**

![image](https://user-images.githubusercontent.com/110905426/208236579-4c0d0e6c-d1fc-4703-8fcc-58b579ea124c.png)



            




