# Windows Server Post-Installation Configuration

After installing Windows Server, the server is not yet ready for production use or to install additional roles and software. You must complete the **post-installation configuration** tasks to properly prepare the server.

The quickest and safest way is to follow a **checklist**. Below is a complete, clean, and well-organized Windows Server post-installation checklist.

## Windows Server Post-Installation Checklist

- [ ] Do not start Server Manager automatically at logon  
- [ ] Turn off Internet Explorer Enhanced Security Configuration  
- [ ] Set time zone  
- [ ] Change computer name  
- [ ] Check and rename network card  
- [ ] Configure static IP address  
- [ ] Turn on Remote Desktop  
- [ ] Configure Windows Firewall to allow pings (ICMP)  
- [ ] Update Windows Server  
- [ ] Activate Windows Server  

---

### 1. Do not start Server Manager automatically at logon

By default, **Server Manager** launches every time you log on. This is unnecessary for most administrators.

**Steps:**
1. Open **Server Manager**.
2. Click **Manage** → **Server Manager Properties**.
3. Check the box: **Do not start Server Manager automatically at logon**.
4. Click **OK**.
<img width="1026" height="768" alt="2026-03-25 09_14_10-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/c93e7f2e-3038-4f60-b73a-24c11ddaee5c" />


**For Domain Controllers (Recommended):**  
Create a **Group Policy Object (GPO)** to disable Server Manager auto-start and link it to the appropriate Organizational Unit (OU).


---

### 2. Turn off Internet Explorer Enhanced Security Configuration

IE Enhanced Security Configuration is very restrictive and often causes issues when downloading software or accessing websites.

**Steps:**
1. Open **Server Manager**.
2. Click **Local Server** in the left pane.
3. Next to **IE Enhanced Security Configuration**, click **On** for both **Administrators** and **Users**.
4. Set both to **Off**.
5. Click **OK**.

<img width="1033" height="768" alt="2026-03-25 09_19_43-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/f6901002-8dc3-488d-aee9-89d1f016d50d" />


### 3. Set Time Zone

Correct time zone is essential, especially for Domain Controllers and Kerberos authentication.

**Steps:**
1. Right-click the clock on the taskbar → **Adjust date and time**.
2. Click **Change time zone**.
3. Select the correct time zone for your location.
4. Click **OK**.

> **Note:** If the time does not update correctly, ensure the server has internet access or manually set the correct time.

<img width="1032" height="768" alt="2026-03-25 09_23_57-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/17a2cdee-033f-40a6-89f0-e31ee3cbefbb" />


### 4. Change Computer Name

Give the server a meaningful and identifiable name (especially important for Domain Controllers).

**Example:** `DC01-2019`, `FS01-PROD`, `APP02-2022`

**Steps:**
1. Open **Server Manager** → **Local Server**.
2. Click on the current computer name.
3. Click **Change…**.
4. Enter the new **Computer name**.
5. Click **OK**.
6. Restart the server when prompted.

<img width="1025" height="768" alt="2026-03-25 09_26_08-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/8a131864-ebb3-423e-af8d-e110654c8e2d" />


### 5. Check and Rename Network Card

**Steps:**
1. Open Command Prompt as Administrator and run:
   ```cmd
   ipconfig /all


Or use PowerShell:
PowerShellGet-NetIPConfiguration
<img width="1025" height="768" alt="2026-03-25 09_28_37-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/a58d8091-29bb-45ba-829a-8973202e0688" />

Rename the network adapter to something identifiable:
Example: Network 192x (if using 192.168.x.x network)


To rename:

Go to Network and Sharing Center → Change adapter settings.
Right-click the adapter → Rename.

<img width="1025" height="768" alt="2026-03-25 09_29_31-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/9ec22584-4384-4c28-a827-0d364ebc047e" />


6. Configure Static IP Address
Steps:

Right-click the network adapter → Properties.
Select Internet Protocol Version 4 (TCP/IPv4) → Properties.
Select Use the following IP address and fill in:
IP address
Subnet mask
Default gateway
Preferred DNS server (e.g., 8.8.8.8 or your internal DNS)
Alternate DNS server (e.g., 8.8.4.4)

Click OK on all windows.

<img width="1022" height="768" alt="2026-03-25 09_32_36-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/01a57e64-1b7c-478d-a9f3-4ce8263b11b7" />

7. Turn on Remote Desktop
Steps:

Open Server Manager → Local Server.
Click Remote Desktop (currently set to Disabled).
Select Allow remote connections to this computer.
(Optional) Click Select Users to add specific users or groups.
Click OK.

<img width="409" height="466" alt="2026-03-25 09_35_05-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/87a74f3e-69eb-47f4-a725-8d923f4258aa" />

8. Configure Windows Firewall to Allow Pings
It is recommended to keep the Windows Firewall enabled. Instead of disabling it, allow ICMP echo requests (ping).
Steps:

Search for and open Windows Defender Firewall with Advanced Security.
In the left pane, click Inbound Rules.
Find and enable the following two rules:
File and Printer Sharing (Echo Request – ICMPv4-In)
File and Printer Sharing (Echo Request – ICMPv6-In)

<img width="1016" height="723" alt="2026-03-25 09_37_27-Windows Server 2019 - VMware Workstation" src="https://github.com/user-attachments/assets/c745fe0a-4cf5-4cae-954f-acca5589a1b4" />


9. Update Windows Server
Keeping the server updated is critical for security and stability.
Steps:

Go to Settings → Update & Security → Windows Update.
Click Check for updates.
Install all available updates.
Restart the server if required.
Repeat the process until you see You're up to date.
<img width="1367" height="711" alt="Up to date" src="https://github.com/user-attachments/assets/28161054-426a-46a8-bba1-f382dbf03e3a" />


10. Activate Windows Server
Activate Windows Server using one of the available licensing methods (KMS, MAK, or your organization's licensing server).
Common commands (run in elevated Command Prompt):
cmdslmgr /dli          # Check activation status
slmgr /ato          # Activate online

Final Notes

Always use a checklist after installing Windows Server to avoid missing critical configuration steps.
For production servers, document the final configuration (IP, hostname, roles, etc.).
Consider creating a standardized post-installation script for repeated deployments.


Documented By:
Simon Etim
