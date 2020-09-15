# Brooklyn Nine Nine

[Room Link](https://tryhackme.com/room/brooklynninenine)

## Enumeration
Using rustscan on the IP we can find 3 open ports: 21 (FTP), 22 (SSH), 80 (HTTP)

## Way 1: Stego with Holt

## Stego
In the website you can find this comment:
>Have you ever heard of steganography?

If you've never heard of steganography, it's about hiding files in other files, for example text in an image.
We can download the background image of the website at http://[ip]/brooklyn99.jpg.
We can try to get the data out of the image using steghide:

```sh
steghide extract -sf brooklyn99.jpg
```

Sadly enough we need a passphrase and it's not just blank, luckily for us there exists a tool called stegcracker that helps with cracking these passphrases.

```sh
stegcracker brooklyn99.jpg /opt/wordlists/rockyou.txt
```
This tells us the passphrase is "admin", it also automatically extracts what was hidden in the file in brooklyn99.jpg.out
Inside the Brooklyn99.jpg file there was a hidden textfile containing the following:
>Holts Password:
>fluffydog12@ninenine

## SSH
Now we have credentials that can be used with SSH, let's connect to it.
First thing we should do is get the user flag, it's in Holt's homedirectory.
We can get linpeas on the system but lets check some common attack vectors first such as sudo privileges.

>User holt may run the following commands on brookly_nine_nine:
>    (ALL) NOPASSWD: /bin/nano


Something we can run with sudo, alright let's check GTFObins for a privilege escalation using this.
Luckily for us there is a way to privesc the system using sudo and nano it goes like this:

```sh
sudo nano
^R^X						# These are your inputs in nano, doing these will get you to execute command mode
reset; sh 1>&0 2>&0			# This is the command you'll be executing
```

Just like that we're root, we can get the root flag and be done.

That's one way of rooting the box but there is a second way to do it without doing the steganography, in the second part of this writeup I'll show you how to do that.

# FTP with Jake

## FTP
Let's connect to the FTP server, we saw this with rustscan/nmap.
It needs a login but don't wory the Anonymous user is enabled so we're safe.
In here is a note to jake, it says

>From Amy,
>Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine

Weak password? Seems like a way in to me, let's see what hydra can do

```sh
hydra -l jake -P /opt/wordlists/rockyou.txt ssh://10.10.193.254
```

The password is 987654321 and we have a way into the system now, thanks Jake!

## SSH
Using the previously found credentials we can log into the system.
We can upload linpeas to the device but we should do some simple manual checks first (or at least while linpeas is running).
Let's do the simplest one first, checking sudo privileges

>User jake may run the following commands on brookly_nine_nine:
>    (ALL) NOPASSWD: /usr/bin/less

We see a sudo privilege, let's see if we can find some way to exploit this on GTFObins.
Of course there is a way of doing this, it works like this:

```sh
sudo less /etc/profile
!/bin/sh
```

And that's it, you're root now, go snag that root flag. If you didn't read the previous part you should be able to find the user flag in the user holt his homefolder

## Conclusion
This was a pretty simple box and it was really cool to see other ways of aproaching this box, while exploring I also found a homedirectory for Amy so I wonder if there was some way of getting into her account.

