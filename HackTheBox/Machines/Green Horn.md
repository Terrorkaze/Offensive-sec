# **Scanning**
nmap -sC -sV 10.10.11.25 -Pn -oN GreenHorn
the scan results are in the file below
![[HackTheBox/Machines/GreenHorn]]


in the scan i noticed it Did not follow redirect to http://greenhorn.htb/ so i added it to the /etc/hosts

i checked out the ports  ,started with port 80 i just browsed through ,then went to port 3000 ,browsed through and this directory/file was intresting
http://greenhorn.htb:3000/GreenAdmin/GreenHorn/src/branch/main/data/settings/pass.php 
i found a hashed password and by reading through other files i found out this was sha512.
I cracked the hash with ==crackstation==  NOTE:" If doing a pentest cracking hashes with online tools is considered breach of contract"
But used that because i couldn't use hashcat properly.

From those files i found out that the is an admin page and that password was used to login there.
the webserver was running pluck 4.7.18 

# Exploitation
I searched for exploit relating to the version of pluck that was running.
There alot but most of the ones i found were not working for me i tried editing the scripts but no luck. i SEARCHED again till i found this one https://github.com/b0ySie7e/Pluck_Cms_4.7.18_RCE_Exploit this one was easy to use. 
## Gaining Access
i used it to get a reverse shell to the webserver.
after gaining access i tried the password from before to elevate to another user and it worked ,i was able to access the ==user.txt (flag)==

# Privilege escalation (Root access)
In the home directory of the user i switched to there was a pdf file, i tried extracting strings but there where not really that useful so i downloaded it over to my local machine.
In it there was part which  was reducted and it was the password.
i researched how to extract images from a pdf and i found i could use this tool `pdfimages` .I then searched how to recover plaintext from pixelized screenshots and i found this tool https://github.com/spipm/Depix. it manged to extract the password but it wasn't perfectly clear so i had to guess there and there.

I used the recovered to password login to ssh as root (i accidentally closed the reverse shell), then i  read the root.txt(flag)


Overall it wasn't a hard a machine the scripts where the ones giving me a hard time there and there.  
