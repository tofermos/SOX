---
title: "PowerShell. Configuració del DNS en un Domini Windows Server"
author: '@tofermos'
date: "2024-10-12"
---
# PowerShell. Configuració del DNS en un Domini Windows Server

## 1 Requisits previs


* Comprovar si el rol DNS està instal·lat

```powershell
Get-WindowsFeature -Name DNS
```

* Comprovar si el servei DNS està actiu

```powershell
Get-Service -Name DNS
```
* Per comprovar si  funciona correctament. Mostra les zones DNS configurades si el servei **DNS** està **instal·lat i funcionant** correctament.

```powershell
Get-DnsServerZone
```

A continuació, et presente la guia completa sobre el servei **DNS en Windows Server 2019**, fusionant la descripció original amb els **cmdlets de PowerShell** per gestionar cada apartat. Això et permetrà realitzar la gestió tant des de la interfície gràfica com des de PowerShell.


## 2 Guia sobre el servei DNS en un Servidor de Domini Windows Server 2019 amb Active Directory

El servei **DNS (Domain Name System)** és essencial en una xarxa basada en un servidor de domini amb **Active Directory**, ja que tradueix els noms de domini en adreces IP. 
Això possibilita la comunicació dins de la xarxa local i fora (internet). 


### 2.1. Funció del DNS en un servidor de domini

El DNS en un servidor amb **Active Directory** té les funcions següents:
- **Resoldre noms de domini** com `ServidorWS1.SMX2B.local` en adreces IP com 192.168.100.1.
- **Emmagatzemar i gestionar registres DNS** per als dispositius dins del domini.
- Permetre als clients del domini **trobar recursos essencials** com DC (controladors de domini), servidors DHCP, etc. 

### 2.2. Verificació del servei DNS

Després de configurar **Active Directory**, el servei DNS hauria d'estar correctament configurat.

#### Verifiquem des del GUI
1. Obri el **Server Manager** i selecciona **Tools** → **DNS**
2. Revisa que, a l'esquerra, aparega el teu servidor DNS i les zones de cerca directa i inversa.

#### Verifiquem amb Cmdlet. Llistant les zones DNS existents

Utilitza aquest cmdlet per llistar totes les zones configurades de cerca directa i inversa configurades al servidor DNS.

```powershell
Get-DnsServerZone
```

### 2.3. Zones DNS

El DNS organitza els registres dins de **zones**. Hi ha dos tipus principals de zones:
- **Zona de cerca directa (Forward Lookup Zone)** Tradueix noms de domini a adreces IP.
- **Zona de cerca inversa (Reverse Lookup Zone)** Tradueix adreces IP a noms de domini.

#### GUI: Crear una zona de cerca directa
1. Al **DNS Manager**, fes clic dret sobre **Forward Lookup Zones**.
2. Selecciona **New Zone...** i segueix l'assistent per crear la zona.

#### Cmdlet per crear una zona de cerca directa
Per crear una nova zona de cerca directa mitjançant PowerShell:

```powershell
Add-DnsServerPrimaryZone -Name "domini.local" -ZoneFile "domini.local.dns"
```

#### GUI: Crear una zona de cerca inversa
1. Al **DNS Manager**, fes clic dret sobre **Reverse Lookup Zones**.
2. Selecciona **New Zone...** i segueix l'assistent, especificant l'interval d'adreces IP per a la teua xarxa.

#### Cmdlet per crear una zona de cerca inversa
Per crear una zona de cerca inversa amb PowerShell:

```powershell
Add-DnsServerPrimaryZone -NetworkId "192.168.100.0/24" -ZoneType Primary -ReplicationScope Domain
```

---

### 3. Tipus de registres DNS comuns

Els **registres DNS** permeten associar noms de dispositius amb adreces IP o altres tipus d'informació. Alguns dels registres més utilitzats són:
- **A (Host record)** Associa un nom de domini amb una adreça IPv4.
- **PTR (Pointer record)** S'utilitza en zones de cerca inversa per associar una adreça IP amb un nom de domini.
- **CNAME (Alias record)** Crea un àlies per a un altre nom de domini.
- **MX (Mail Exchange record)** Indica el servidor de correu associat a un domini.

#### GUI: Crear un registre A (Host)
1. Al **DNS Manager**, dins de la teua **zona de cerca directa**, fes clic dret i selecciona **New Host (A or AAAA)**.
2. Introduïx el nom (per exemple, `pc-client1`) i l'adreça IP.
3. Fes clic a **Add Host**.

#### Cmdlet per crear un registre A (Host)
Per crear un registre A amb PowerShell:

```powershell
Add-DnsServerResourceRecordA -Name "pc-client1" -ZoneName "domini.local" -IPv4Address "192.168.100.10"
```

#### GUI: Crear un registre PTR (per a la cerca inversa)
1. Al **DNS Manager**, dins de la teua zona de cerca inversa, fes clic dret i selecciona **New Pointer (PTR)**.
2. Introduïx l'adreça IP i el nom de domini al qual apuntarà.

#### Cmdlet per crear un registre PTR
Per crear un registre PTR amb PowerShell:

```powershell
Add-DnsServerResourceRecordPtr -Name "192.168.100.10" -ZoneName "100.168.192.in-addr.arpa" -PtrDomainName "pc-client1.dominio.local"
```

---

### 4. Configurar els clients perquè utilitzen el servidor DNS

Els clients del domini han d'utilitzar el **servidor DNS** per poder resoldre correctament els noms dins del domini. El servidor **DHCP** pot configurar automàticament els clients per utilitzar el DNS. També es pot configurar manualment.

#### GUI: Configurar manualment el DNS en un client (Windows 10)
1. A **Configuració de xarxa**, selecciona la connexió activa (Ethernet o Wi-Fi).
2. A **Propietats**, selecciona **TCP/IPv4** i configura l'adreça IP del servidor DNS (normalment, el servidor de domini).

#### Cmdlet per veure la configuració del servidor DNS en un client
Per veure els servidors DNS configurats en un client:

```powershell
Get-DnsClientServerAddress -InterfaceAlias "Ethernet"
```

#### Cmdlet per configurar un servidor DNS en un client
Per configurar l'adreça del servidor DNS en un client:

```powershell
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses ("192.168.100.100")
```

---

### 5. Verificar que el DNS funciona correctament

Pots comprovar que el DNS funciona correctament utilitzant la comanda `nslookup` en un PC client.

#### GUI: Utilitzar **nslookup
1. Obri el símbol del sistema (CMD) en un PC client.
2. Escriu:

   ```cmd
   nslookup nomServidor.domini.local
   ```

Això ha de retornar l'adreça IP associada al nom de domini.

#### Cmdlet per fer una consulta DNS amb PowerShell
Per fer una consulta DNS similar a `nslookup` utilitzant PowerShell:

```powershell
Resolve-DnsName pc-client1.dominio.local
```

---

### 6. Consultes DNS internes i externes

El servidor DNS pot resoldre noms dins del domini i també enviar consultes a altres servidors DNS per a dominis externs (internet). Això es fa configurant **reenviadors (forwarders)**.

#### GUI: Configurar reenviadors DNS
1. Al **DNS Manager**, fes clic dret sobre el teu servidor DNS i selecciona **Properties**.
2. A la pestanya **Forwarders**, afegix els servidors DNS externs (per exemple, `8.8.8.8`).

#### Cmdlet per configurar reenviadors DNS
Per afegir reenviadors DNS utilitzant PowerShell:

```powershell
Add-DnsServerForwarder -IPAddress "8.8.8.8"
```

---

# 3 Consultes, estadístiques i neteja de búsquedes.

### Consulta, eliminació i estadística de consultes DNS (Des del client i des del servidor)

A continuació tens un **resum fusionat** de les operacions més importants per a la gestió de les consultes DNS, tant des d'un **client Windows 10** com des del **servidor DNS** en un entorn de Windows Server 2019.

## Des del **Client Windows 10**

### 1. **Veure la cache DNS del client**
El **client** guarda en la seua cache les consultes DNS que ha realitzat anteriorment. Pots consultar la cache amb:

```powershell
Get-DnsClientCache
```

### 2. **Netejar la cache DNS del client**
Per esborrar la cache DNS del **client** i forçar que es resolguen de nou les consultes DNS:

```powershell
Clear-DnsClientCache
```

```cmd
ipconfig /flushdns
```

### 3. **Consultar un nom DNS (similar a `nslookup`)**
Pots fer una consulta DNS directament des del client per verificar la resolució d'un nom:

```powershell
Resolve-DnsName nomdelpc.dominio.local
```

Aquestes comandes s'executen localment en un client Windows 10 sense necessitat d'accés al servidor.

---

## Des del **Servidor DNS**

### 1. **Veure la cache DNS del servidor**
El **servidor DNS** emmagatzema les consultes resoltes recentment en una cache per millorar el rendiment. Per veure la cache del servidor DNS:

```powershell
Get-DnsServerCache
```

Aquesta comanda et mostrarà les entrades de cache del servidor DNS, incloent les consultes resoltes.

### 2. **Veure estadístiques de consultes DNS**
Per veure les **estadístiques DNS** que inclouen el nombre total de consultes, errors, reenviaments, etc.:

```powershell
Get-DnsServerStatistics
```

Això et permet monitorar l'activitat del servei DNS i detectar problemes potencials.

### 3. **Activar el registre de consultes DNS (logging)**
Si vols registrar totes les consultes DNS que es realitzen al servidor, has d'activar el registre de consultes (debug logging). Pots fer-ho amb la següent comanda:

```powershell
Set-DnsServerDiagnostics -EnableLogging $true
```

Les consultes es registraran en un fitxer, el qual podràs revisar posteriorment per auditar les consultes realitzades.



## Des del **Client Windows 1x** amb **PowerShell remota**

Si tens accés administratiu al servidor, pots executar comandes de servidor des d'un client mitjançant **PowerShell remota**. Això permet gestionar el DNS remotament.

### Passos per gestionar el DNS remotament:
1. **Iniciar una sessió remota** al servidor DNS des del client:

   ```powershell
   Enter-PSSession -ComputerName nomdelservidor
   ```

2. Executa comandes com si estigueres en el servidor, per exemple:

   ```powershell
   Get-DnsServerCache
   ```

3. **Sortir de la sessió remota**:

   ```powershell
   Exit-PSSession
   ```

---


### 9. Monitoreig del servei DNS

És important monitorar el servei DNS per assegurar-te que està funcionant correctament. Pots utilitzar l'**Event Viewer** per veure esdeveniments relacionats amb el DNS.

#### GUI: Veure esdeveniments de DNS
1. Al **Server Manager**, selecciona **Tools** → **Event Viewer**.
2. Ves a **Applications and Services Logs** → **DNS Server** per veure els esdeveniments relacionats amb el DNS.

#### Cmdlet per veure registres d'esdeveniments de DNS
Per veure esdeveniments relacionats amb el DNS utilitzant PowerShell:

```powershell
Get-WinEvent -LogName "DNS Server"
```

## Resum general

- **Des del client Windows 1x**:
  - Consulta la cache DNS amb `Get-DnsClientCache`.
  - Neteja la cache DNS amb `Clear-DnsClientCache`.
  - Consulta noms DNS amb `Resolve-DnsName`.

- **Des del servidor DNS**:
  - Veure la cache DNS amb `Get-DnsServerCache`.
  - Veure estadístiques de DNS amb `Get-DnsServerStatistics`.
  - Activar el registre de consultes DNS amb `Set-DnsServerDiagnostics -EnableLogging $true`.
