# Description
We often find credentials in network shares within scripts and configuration files (batch, cmd, PowerShell, conf, ini, and config). In contrast, credentials on a user's local machine primarily reside in text files, Excel sheets, or Word documents. The main difference between the storage of credentials on shares and machines is that the former poses a significantly higher risk, as it may be accessible by every user. A network share may be accessible by every user for four main reasons:

One admin user initially creates the shares with properly locked down access but ultimately opens it to everyone. Another admin of the server could also be the culprit. Nonetheless, the share eventually becomes open to Everyone or Users, and recall that a server's Users group contains Domain users as its member in Active Directory environments. Therefore every domain user will have at least read access (it is wrongly assumed that adding 'Users' will give access to only those local to the server or Administrators).
The administrator adding scripts with credentials to a share is unaware it is a shared folder. Many admins test their scripts in a scripts folder in the C:\ drive; however, if the folder is shared (for example, with Users), then the data within the scripts is also exposed on the network.
Another example is purposely creating an open share to move data to a server (for example, an application or some other files) and forgetting to close it later.
Finally, in the case of hidden shares (folders whose name ends with a dollar sign $), there is a misconception that users cannot find the folder unless they know where it exists; the misunderstanding comes from the fact that Explorer in Windows does not display files or folders whose name end with a $, however, any other tool will show it.

## Attack

The first step is identifying what shares exist in a domain. There are plenty of tools available that can achieve this, such as PowerView's `Invoke-ShareFinder`.
```bash
Invoke-ShareFinder -domain eagle.local -ExcludeStandard -CheckShareAccess
```
![image](https://github.com/offensivecyber03/Windows-Attack-Defense/assets/71892943/d89eddc9-eab9-466d-9b96-ada342269678)
<br>
The playground environment has few shares in the domain, however, in production environments, the number can reach thousands (which requires long periods to parse).

The image above shows a share with the name dev$. Because of the dollar sign, if we were to browse the server which contains the share using Windows Explorer, we would be presented with an empty list (shares such as C$ and IPC$ even though available by default, Explorer does not display them because of the dollar sign):
