# vapt-reference-mats
For VAPT Final Exam

## Additional References:

https://github.com/The-Art-of-Hacking/h4cker

https://github.com/swisskyrepo/PayloadsAllTheThings

https://youtu.be/K7y_-JtpZ7I?si=EUvnqTgaoFKlX8ri

https://youtu.be/QynUOJanNqo?si=S-24aBcBqHl_Ava3

## Metasploit references:

https://www.comparitech.com/net-admin/metasploit-cheat-sheet/

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Metasploit%20-%20Cheatsheet.md

https://www.offsec.com/metasploit-unleashed/

https://medium.com/@ojovbo.nelson/metasploit-guide-for-beginners-part-1-5dc04aa6ac05

## SQLMAP

https://medium.com/cybersecurity-101/exploring-sql-injection-with-sqlmap-a-practical-guide-09e56e04d00f

https://medium.com/@cuncis/the-ultimate-sqlmap-tutorial-master-sql-injection-and-vulnerability-assessment-4babdc978e7d

## Netcat details

https://www.geeksforgeeks.org/practical-uses-of-ncnetcat-command-in-linux/

Random command to help find vulnerabilities:

`find / -perm -u=s -type f 2>/dev/null`

After doing netcat for reverse shell, if python is present try this to get proper shell:

```
tty
python -c "import pty; pty.spawn('/bin/bash')"
```
