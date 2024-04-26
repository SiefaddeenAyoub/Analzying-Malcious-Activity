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

13. 






