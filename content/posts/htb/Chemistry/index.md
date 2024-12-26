+++
title = 'Chemistry'
Topics = ['easy','linux']
date = 2024-11-10T14:15:04Z
draft = true
+++
![chemistry.png](/chemistry/chemistry.png)

# User
![nmap.png](/chemistry/nmap.png)

![upnp.png](/chemistry/upnp.png)

![cif exploit](/chemistry/cif.png)
![cif exp](/chemistry/cifexploit.png)


![exploited + in as app.png](/chemistry/exploitedapp.png)


![sqlite connected](/chemistry/sqlliteconnected.png)

![hashes](/chemistry/hashes.png)

```
SELECT * FROM user;
1|admin|2861debaf8d99436a10ed6f75a252abf
2|app|197865e46b878d9e74a0346b6d59886a
3|rosa|63ed86ee9f624c7b14f1d4f43dc251a5
4|robert|02fcf7cfc10adc37959fb21f06c6b467
5|jobert|3dec299e06f7ed187bac06bd3b670ab2
6|carlos|9ad48828b0955513f7cf0f7f6510c8f8
7|peter|6845c17d298d95aa942127bdad2ceb9b
8|victoria|c3601ad2286a4293868ec2a4bc606ba3
9|tania|a4aa55e816205dc0389591c9f82f43bb
10|eusebio|6cad48078d0241cca9a7b322ecd073b3
11|gelacia|4af70c80b68267012ecdac9a7e916d18
12|fabian|4e5d71f53fdd2eabdbabb233113b5dc0
13|axel|9347f9724ca083b17e39555c36fd9007
14|kristel|6896ba7b11a62cacffbdaded457c6d92
15|test|098f6bcd4621d373cade4e832627b4f6
16|user|1a1dc91c907325c69271ddf0c944bc72
17|username|5f4dcc3b5aa765d61d8327deb882cf99
18|root|63a9f0ea7bb98050796b649e85481845
19|teste|698dc19d489c4e4db73e28a713eab07b
20|andrew|58d613129c5e71de57ee3f44c5ce16bc
21|'1+1|af15d5fdacd5fdfea300e88a8e253e82
22|a|0cc175b9c0f1b6a831c399e269772661
23|b|92eb5ffee6ae2fec3ad71c777531578f
24|ropot|513ba0dd75b023eee63163b61ae4bc3b
25|admin24|a577e0ad6296e88fb1898ecace3cd043
26|soares|87aaa9a4370765e680bd581d14e089ad

```


![unicornrosadosa.png](/chemistry/unicornrosadosa.png)


or


![hashcat.png](/chemistry/hashcat.png)

```
rosa : unicorniosrosados

```

![user](/chemistry/user.txt.png)

# Root

ran linpeas tried few exploits nothing worked


checked internal ports

![internal](/chemistry/internal.png)
```
netstat -ano | grep 127
netstat -ano | grep 192
```



![port forward](/chemistry/port.png)
```
ssh -L 127.0.0.1:8080:127.0.0.1:8080 rosa@10.10.11.38
```


![whatweb](/chemistry/whatweb.png)
```
whatweb {ip}
```


![poc](/chemistry/poc.png)
Found [this poc](https://github.com/z3rObyte/CVE-2024-23334-PoC)


![static not workinh.png](/chemistry/static.png)

so static wasnt wokring becuase
![notadir.png](/chemistry/notadir.png)
static wasnt a directory



![foundassets.png](/chemistry/foundassets.png)
so i found assets which is funny becauseee....
![dirsearch](/chemistry/dirsearch.png)
i had already found it but overlooked it because it was forbidden

![changed to assset.png](/chemistry/changed.png)

so i changed the payload to assets/ and.... IT WORKED

so then i thought "Hmmm what can i do with this"

then it popped into my head "just get the root.txt"

![root.txt](/chemistry/root.txt.png)

and so i did !!!

![pwnd](/chemistry/pwned.png)

good box i enjoyed it

Thanks for reading
