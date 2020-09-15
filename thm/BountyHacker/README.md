# Bounty Hacker
[Room Link](https://tryhackme.com/room/cowboyhacker)

## Enumeration
Using rustscan we can see 3 open ports: 21 (FTP), 22 (SSH) and 80 (HTTP)

## FTP
Port 21 is FTP, we can connect to it and log in using the Anonymous account (Anonymous and blank password).
When we connect to ftp we can find 2 files: locks.txt and tasks.txt, we download these to our computer using get.
Reading locks.txt shows us a dictionary of passwords, checking tasks.txt shows us our next task.

>1.) Protect Vicious.
>2.) Plan for Red Eye pickup on the moon.
>-lin

These give us 2 potential usernames: Vicious and lin, since our password dictionary is so small we can easily bruteforce both of them with Hydra.
After bruteforcing a login can be found for lin: RedDr4gonSynd1cat3

```sh
hydra -l lin -P locks.txt ssh://[ip]:22
```

## SSH
Now we have a login that works with SSH so let's connect to it.

First we should look for the user flag, you can run a find command for this or you can manually look for it since there are very little folders, the user flag can be found in ~/Desktop

Next up we need to get the root flag, at first I tried to use linpeas on the system but it was super slow so I did some manual checking.
After some trial and error I decided to check for sudo privileged using `sudo -l`, this showed me that lin can run tar wi
Now we have an attack vector, checking GTFObins shows us that we can privesc using sudo and tar by running the following command:

```sh
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

There you go, you are now root. Snag the flag at /root/ and you have completed the box!

# Conclusion
This was a very easy box but even easy boxes can teach you something, for me this box tought me to do more manual checking for potential privesc.
As mentioned linpeas was running very slow, I could have saved a lot of time and trouble by just manually checking some simple things before moving onto linpeas.
