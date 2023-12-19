# AS-REProasting

The AS-REProasting attack is similar to the Kerberoasting attack; we can obtain crackable hashes for user accounts that have the property `Do not require Kerberos preauthentication` enabled. The success of this attack depends on the strength of the user account password that we will crack.

## Attack
To obtain crackable hashes, we can use `Rubeus` again. However, this time, we will use the `asreproast` action. If we don't specify a name, Rubeus will extract hashes for each user that has Kerberos preauthentication not required:
```bash
PS C:\Users\bob\Downloads> .\Rubeus.exe asreproast /outfile:asrep.txt
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/d1e00535-b8c0-42e4-9f7b-7070c5610be3)

```bash
NOTE: Once Rubeus obtains the hash for the user Anni (the only one in the playground environment with preauthentication not required), we will move the output text file to a linux attacking machine.
For hashcat to be able to recognize the hash, we will use hashcat 18200 mode and we need to edit it by adding 23$ after $krb5asrep$:
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/1a2ce6c0-97be-4b6a-8cb4-459ffd526daf)<br>


