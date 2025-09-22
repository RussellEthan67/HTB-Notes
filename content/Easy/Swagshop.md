### Enumeration
- Nmap![[Pasted image 20250918170820.png]]
- Gobuster port 80![[Pasted image 20250918173013.png]]
- To find exploit, first we must find the version of this Magento. There are a few hints we can take like this one
- The copyright date have a high chance to be the correct indicator of when this CMS is set up ![[Pasted image 20250919094906.png]]
- Found in common path for magento server ![[Pasted image 20250919095258.png]]
- Based from these 2, i think we can try to find exploit for magento server that is old

### Exploitation
- Now, we got to look for the right exploit. Facing CMS like this, we can find the spesific CMS scanner to reliably find the correct CMS version. I found one [here](https://github.com/steverobbins/magescan) : ![[Pasted image 20250919103251.png]]
- From running the scanner, we can find the correct version of Magento CMS![[Pasted image 20250919103710.png]]
- Now, we can search in searchsploit![[Pasted image 20250919103742.png]]
	- In the context of HTB and CTF, we are interested in RCE-type of exploit, which means there are 2 interesting exploit we can try. But since 1 of them needs to be authenticated, we can try Magento eCommerce - Remote Code Execution one
- This is the exploit, since we can see that this exploit is made in 2015, this have a chance of working, we just need to modify to suit our need![[Pasted image 20250919104137.png]]
- Now, we can run it![[Pasted image 20250919105511.png]]
- And log in to our fresh admin account![[Pasted image 20250919105542.png]]
- Nice! Now we can move to the next step

### Exploitaiton (Next Step)
- Now that we get an authenticated account, if we remember from our searchsploit result earlier![[Pasted image 20250919105906.png]]
	- There is another valid RCE exploit. Now we can try that
- This it the exploit![[Pasted image 20250919110055.png]]
	- Extra information i found on 0xdf walkthrough : 
		- "For background on this bug, it’s a PHP Object Injection vulnerability, detailed by one of the researchers who found it here. PHP Object Injection is a class of bugs that falls under deserialization vulnerabilities. Basically, the server passes a php object into the page, and when the browser submits back to the server, it sends that object as a parameter. To prevent evil users from messing with the object, Magento uses a keyed hash to ensure integrity. However, the key for the hash is the install data, which can be retrieved from `/app/etc/local.xml`. This means that once I have that date, I can forge signed objects and inject my own code, which leads to RCE."
- Again, before using it, we should make some adjustments accordingly![[Pasted image 20250919110307.png]]
	- For date, we can found it here![[Pasted image 20250919110600.png]]
- This is the modified version![[Pasted image 20250919110718.png]]
- Lets run it![[Pasted image 20250919111205.png]]
	- Seems we run into some problem, if you had this problem, all you need to is this : ![[Pasted image 20250919111243.png]]
		- I found this solution in [here](https://forum.hackthebox.com/t/swagshop-rce/1959/3)
- Lets modify it and check if it is working![[Pasted image 20250919111413.png]]
- Well, this one did not work![[Pasted image 20250919112701.png]]
- Looking for solution, i found out you need to add bash -c![[Pasted image 20250919112741.png]]![[Pasted image 20250919112753.png]]
- Alas, we got in. Let's get the user.txt                   ![[Pasted image 20250922120015.png]]  
- Now onto privilege escalation

### Privilege Escalation
- Every time we get shell in Linux system, it is best to upgrade it whenever possible![[Pasted image 20250922120232.png]]
	- Example![[Pasted image 20250922120438.png]]![[Pasted image 20250922120458.png]]
- Simple step to do after getting a foothold is to check what sudo are we allowed by running `sudo -l` ![[Pasted image 20250922120602.png]]
	- vi is a text editor, same as nano
- We can check out [GTFOBins](https://gtfobins.github.io/) on commands we found![[Pasted image 20250922120803.png]] ![[Pasted image 20250922122502.png]]
- Let's try it                                                                            ![[Pasted image 20250922122528.png]]
- Now, just go to root folder and get the root.txt           ![[Pasted image 20250922122633.png]]
