+++
title = 'Blurry'
summary = 'Lorem ipsum'
Topics = ['medium','linux']
date = 2024-09-29T13:49:05Z
draft = false
+++
# Blurry
![lesgo](/Blurry/Blurry.png)

## User

![nmap](/Blurry/nmap.png)

![clearml](/Blurry/clearml.png)

I tried a random name "admin" and got access to the dashboard

![dashoard](/Blurry/dashboard.png)

![exploit1](/Blurry/exploit1.png)
Found [this](https://github.com/xffsec/CVE-2024-24590-ClearML-RCE-Exploit) exploit


![exploit](/Blurry/exploit.png)
Followed the instructions had to install clearml myself and a few other steps

![foothold](/Blurry/foothold.png)
Then i had foothold and user!!

![user.txt](/Blurry/userwth.png)

## Root
![linpeas](/Blurry/linpeas.png)
I ran linpeas (You could also run "sudo -l" ) and it told me i could run "/usr/bin/evaluate_model /models*.pth" as root

![evil.py](/Blurry/evil.py.png)
I created this python file that created a .pth file and all the .pth file did was cat root.txt

![root.txt](/Blurry/root.txt.png)
Then we got the root.txt

![pwnd](/Blurry/pwned.png)
And thats pwnd.

Thank you for reading
