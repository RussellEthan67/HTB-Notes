Nmap scan![[Pasted image 20250827191050.png]]
- It looks like Active Directory
### SMB Enumeration
- Since this is windows machine, check SMB first using SMBMap![[Pasted image 20250827223657.png]]
- Use -r flag and depth to find interesting file![[Pasted image 20250827230335.png]]![[Pasted image 20250827230359.png]]
	- In the context of AD, Groups.xml is important file?
	- Get the Groups.xml![[Pasted image 20250827230640.png]]![[Pasted image 20250827230709.png]]
	- Decrypt it with gpp-decrpyt![[Pasted image 20250827231259.png]]
### SMB with user password
- Try SMB again, now with password![[Pasted image 20250827231819.png]]
- Found the user.txt in Users share![[Pasted image 20250827231731.png]]

### Kerberoasting
- Since we have a valid account, we can use kerberoasting to try gain admin on AD![[Pasted image 20250827232350.png]]
- Crack it using hashmap![[Pasted image 20250827233736.png]]

### SMB, but with Admin Account
- SMBMap![[Pasted image 20250827234252.png]]
- Get the root.txt![[Pasted image 20250827234232.png]]

Lessons
- The flag seems always on Dekstop of the user