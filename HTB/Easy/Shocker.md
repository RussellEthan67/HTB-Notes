- Nmap Scan![[Pasted image 20250903234810.png]]
- Port 80![[Pasted image 20250903234922.png]]![[Pasted image 20250903234933.png]]
- Dirsearch
	`dirsearch -u http://10.10.10.56/ -r -e php,txt,bak,sh`![[Pasted image 20250904095935.png]]

- User.sh found in cgi-bin![[Pasted image 20250910063154.png]]
- Shellshock proof of concept![[Pasted image 20250910080742.png]]
- Getting a shell![[Pasted image 20250910081220.png]]
- Upgrade to full shell![[Pasted image 20250910081335.png]]
- Finding user.txt![[Pasted image 20250910081500.png]]
- Priv Esc to Root![[Pasted image 20250910104108.png]]
- Root flag![[Pasted image 20250910104232.png]]
- 