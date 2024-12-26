+++
title = 'Boardlight'
Topics = ['easy','linux']
date = 2024-09-21T14:15:04Z
draft = false
+++
# Boardlight
![boardlight](/Boardlight/BOARDLIGHT.png)

## User

![nmap](/Boardlight/NMAP.png)


![WEB](/Boardlight/WEB.png)

FFuF was doing this

![ffuf error](/Boardlight/ffuferror.png)


so i added -c -fs 15949



![ffuf sucess](/Boardlight/ffufsucess.png)
```
ffuf -u http://board.htb -H "Host:FUZZ.board.htb" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -c -fs 15949

```


![login](/Boardlight/login.png)

found a poc for authenticated rev shell so had to get in


Googled default creds which were admin/admin and it worked

made a test page with a rev and ran it

![foothold](/Boardlight/Foothold.png)

got foothold
now onto priv esc

![lariss](/Boardlight/LARISSA.png)


![conf](/Boardlight/conf.php.png)

db name = dolibarr
db username = dolibarrowner
db password = serverfun2$2023!!


larissa password = serverfun2$2023!!

![lariss user.txt](/Boardlight/larissausertxt.png)

## Root


![linpeas suid](/Boardlight/linpeassuid.png)
I ran linpeas and found an unkown SUID binary called Enlightenment

![exploit db](/Boardlight/exploitdb.png)
I googled and found an exploit

![root.txt](/Boardlight/root.txt.png)
I ran the exploit and got a shell as root 

![Pwnd](/Boardlight/PWNed.png)

And thats Pwnd.

Thanks for reading
