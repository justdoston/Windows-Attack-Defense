# Kerberoasting
In Active Directory, a Service Principal Name (SPN) is a unique service instance identifier. Kerberos uses SPNs for authentication to associate a service instance with a service logon account, which allows a client application to request that the service authenticate an account even if the client does not have the account name. When a Kerberos TGS service ticket is asked for, it gets encrypted with the service account's NTLM password hash.

Kerberoasting is a post-exploitation attack that attempts to exploit this behavior by obtaining a ticket and performing offline password cracking to open the ticket. If the ticket opens, then the candidate password that opened the ticket is the service account's password. The success of this attack depends on the strength of the service account's password. 

## Attack Path
To obtain crackable tickets, we can use Rubeus. When we run the tool with the kerberoast action without specifying a user, it will extract tickets for every user that has an SPN registered (this can easily be in the hundreds in large environments):
```bash
.\Rubeus.exe kerberoast /outfile:spn.txt
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/0a448eea-f2cf-4d08-8d37-1a8524a5cd8b)
<br>
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/a4116859-e8f4-4f04-8a5c-fe6eaaa12b51)

We then need to move the extracted file with the tickets to the Kali Linux VM for cracking (we will only focus on the one for the account Administrator, even though Rubeus extracted two tickets)<br>
**We can use hashcat with the hash-mode (option -m) 13100 for a Kerberoastable TGS**
