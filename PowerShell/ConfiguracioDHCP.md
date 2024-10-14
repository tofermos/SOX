---
title: "PowerShell. Configuració del DHCP en un Domini Windows Server"
author: '@tofermos'
date: "2024-10-12"
---
# PowerShell. Configuració del DHCP en un Domini Windows Server

## 1 Requisits previs
Obri una sessió PowerShell amb permisos d'administrador.

### 1.1 Afegeix el rol de DHCP, si no s'ha fet anteriorment:

```powershell
Install-WindowsFeature -Name DHCP -IncludeManagementTools
```

### 1.2 Importa el mòdul de DHCP per gestionar el servei DHCP:

```powershell
Import-Module DhcpServer
```

### 1.3 Configurar la IP fixa del servidor Windows Server 2019

Estableix la següent IP al servidor: 192.168.100.100.
(Per més informació, sobre configuració de la NIC i algunes gestions de xarxa tens un altre apartat en este curs. )

#### Averigua el nom de la NIC i la IP actual.

```powershell
Get-NetIPInterface
```
Suposem una resposta:

```
fIndex InterfaceAlias                  AddressFamily NlMtu(Bytes) InterfaceMetric Dhcp     ConnectionState PolicyStore
------- --------------                  ------------- ------------ --------------- ----     --------------- -----------
1       Loopback Pseudo-Interface 1     IPv6            4294967295              75 Disabled Connected       ActiveStore
7       Ethernet                        IPv4                  1500              25 Disabled Connected       ActiveStore
1       Loopback Pseudo-Interface 1     IPv4            4294967295              75 Disabled Connected       ActiveStore
```

Ja sabem el nom: "Ethernet". També que és fixa ( Dhcp: Disabled). Podem mirar si la IP

```powershell
Get-NetIPAddress -InterfaceAlias "ETHER*"
```

#### Executa la següent ordre per assignar l'IP fixa:

```powershell
Set-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.100.100 -PrefixLength 24
```

#### Assignem l'adreça de loopback com a adreça DNS.

Assumim que, en la nostre domini, el servidor DNS és el mateix que aquest que farà de servidor DHCP. En cas que fora un altre, indicaríem ací la seua IP.

```powershell
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1
```

## 2 Configuració del servei DHCP

### 2.1 Crear un rang d'IPs

Des de la 192.168.100.1 fins a 192.168.100.99:

```powershell
Add-DhcpServerv4Scope -Name "Xarxa Interna" -StartRange 192.168.100.1 -EndRange 192.168.100.99 -SubnetMask 255.255.255.0 -State Active
   ```
#### Creem, consultem i eliminem...
```
PS C:\Windows\system32> Add-DhcpServerv4Scope -Name "Xarxa Interna" -StartRange 192.168.100.1 -EndRange 192.168.100.99 -SubnetMask 255.255.255.0 -State Active
PS C:\Windows\system32> Get-DhcpServerv4Scope

ScopeId         SubnetMask      Name           State    StartRange      EndRange        LeaseDuration
-------         ----------      ----           -----    ----------      --------        -------------
192.168.100.0   255.255.255.0   Xarxa Interna  Active   192.168.100.1   192.168.100.99  8.00:00:00


PS C:\Windows\system32> Remove-DhcpServerv4Scope -ScopeId 192.168.100.0
PS C:\Windows\system32> Get-DhcpServerv4Scope
```

### 2.2 Configuració d'exclusions d'IP

Excloure des de la 192.168.100.80 fins a 192.168.100.99:

1. Executa aquesta comanda PowerShell:

```powershell
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.100.0 -StartRange 192.168.100.80 -EndRange 192.168.100.99
```
Es poden fer més d'una exclusió.

#### Per consultar, eliminar... 

```
PS C:\Windows\system32> Get-DhcpServerv4ExclusionRange -ScopeId 192.168.100.0

ScopeId              StartRange           EndRange
-------              ----------           --------
192.168.100.0        192.168.100.80       192.168.100.99


PS C:\Windows\system32> remove-DhcpServerv4ExclusionRange -ScopeId 192.168.100.0
PS C:\Windows\system32> Add-DhcpServerv4ExclusionRange -ScopeId 192.168.100.0 -StartRange 192.168.100.80 -EndRange 192.168.100.99
```

Si tenim més d'una exclusió...

```
PS C:\Windows\system32> Get-DhcpServerv4ExclusionRange

ScopeId              StartRange           EndRange
-------              ----------           --------
192.168.100.0        192.168.100.80       192.168.100.99
192.168.100.0        192.168.100.20       192.168.100.30


PS C:\Windows\system32> Remove-DhcpServerv4ExclusionRange -ScopeId 192.168.100.0 -StartRange 192.168.100.20 -EndRange 192.168.100.30
PS C:\Windows\system32> Get-DhcpServerv4ExclusionRange

ScopeId              StartRange           EndRange
-------              ----------           --------
192.168.100.0        192.168.100.80       192.168.100.99
```

## 5. Reserva d'IP

Per reservar l'adreça 192.168.100.1 per a un equip amb Windows 11 basant-se en la seua adreça MAC:

### Obtenir la MAC del PC Windows on fer la reserva.

Al Windows 11, obri PowerShell i executa la següent comanda per obtindre la MAC:

```powershell
   Get-NetAdapter | Select-Object Name, MacAddress
```
Pots mirar per l'entorn gràfic.
Si és un altre dispositiu ( no un PC Windows 1x), consulta la documentació tècnica.

### Al servidor executa la següent ordre

```powershell
Add-DhcpServerv4Reservation -ScopeId 192.168.100.0 -IPAddress 192.168.100.1 -ClientId "08-00-27-B5-DC-55" -Description "Reserva per a Windows 11"
```
#### Per consultes, eliminacions...

```
PS C:\Windows\system32> Get-DhcpServerv4Reservation -ScopeId 192.168.100.0

IPAddress            ScopeId              ClientId             Name                 Type                 Description
---------            -------              --------             ----                 ----                 -----------
192.168.100.1        192.168.100.0        08-00-27-b5-dc-55                         Both                 Reserva per...


PS C:\Windows\system32> Remove-DhcpServerv4Reservation -ScopeId 192.168.100.0
PS C:\Windows\system32> Add-DhcpServerv4Reservation -ScopeId 192.168.100.0 -IPAddress 192.168.100.1 -ClientId "08-00-27-B5-DC-55" -Description "Reserva per a Windows 11"
```
Si tenim més d'una reserva:

```
PS C:\Windows\system32> Get-DhcpServerv4Reservation  -ScopeId 192.168.100.0

IPAddress            ScopeId              ClientId             Name                 Type                 Description
---------            -------              --------             ----                 ----                 -----------
192.168.100.3        192.168.100.0        08-00-27-b5-dc-66                         Both                 PC 2 Windows 10
192.168.100.2        192.168.100.0        08-00-27-b5-dc-55                         Both                 PC 1 Windows 11


PS C:\Windows\system32> Remove-DhcpServerv4Reservation -ScopeId 192.168.100.0 -ClientId "08-00-27-b5-dc-55"
PS C:\Windows\system32> Get-DhcpServerv4Reservation  -ScopeId 192.168.100.0

IPAddress            ScopeId              ClientId             Name                 Type                 Description
---------            -------              --------             ----                 ----                 -----------
192.168.100.3        192.168.100.0        08-00-27-b5-dc-66                         Both                 PC 2 Windows 10
```

&#9888;️
Si filtrem el *Remove-DhcpServerv4Reservation* per *-IPAddress 192.168.100.2* elimina totes les reserves del rang.

## 6 Obtenció de la MAC d'altres formes 

#### Des de l'entorn gràfic de Windows 11

1. Fes clic amb el botó dret sobre la icona de xarxa en la barra de tasques i selecciona "Obri Configuració de xarxa i Internet".
2. A la finestra que s'obri, selecciona "Estat" en el panell esquerre, i després selecciona "Canvia les propietats de connexió" de la connexió activa (Wi-Fi o Ethernet).
3. Al final de la pàgina, en l'apartat "Propietats", trobaràs l'adreça MAC sota "Adreça física (MAC)".

#### Amb *ipconfig*

1. Obri el símbol del sistema (CMD) al Windows 11.
2. Executa la següent comanda:

```cmd
ipconfig /all
```
   
