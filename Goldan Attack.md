# Attack
To perform the Golden Ticket attack, we can use `Mimikatz` with the following arguments:
1) `/domain`: The domain's name.
2) `sid`: The domain's SID value.
3) `/rc4`: The password's hash of krbtgt.
4) `/user`: The username for which Mimikatz will issue the ticket (Windows 2019 blocks tickets if they are for inexistent users.)
5) `/id`: Relative ID (last part of SID) for the user for whom Mimikatz will issue the ticket.
6) `/renewmax`: The maximum number of days the ticket can be renewed.
7) `/endin`: End-of-life for the ticket.

Additionally, advanced threat agents mostly will specify values for the `/renewmax` and `/endin` arguments, as otherwise, Mimikatz will generate the ticket(s) with a lifetime of 10 years, making it very easy to detect by EDRs:

First, we need to obtain the password's hash of krbtgt and the SID value of the Domain. We can utilize DCSync with Rocky's account from the previous attack to obtain the hash.
```bash
lsadump::dcsync /domain:eagle.local /user:krbtgt
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/fa31d8cd-8b01-4c6e-8869-ad5d04723062)

