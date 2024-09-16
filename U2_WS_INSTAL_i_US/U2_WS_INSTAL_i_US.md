---
title: U2. Windows Server. Instal·lació i ús
author: "@tofermos 2024"
lang: "ca-ES"
papersize: A4
linestretch: 1.5
output:
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
    df_print: paged
    number_sections: false
  pdf_document:
    toc: true
    keep_tex: true
    latex_engine: xelatex
---

\newpage
\renewcommand\tablename{Tabla}

# 1. Introducció a Windows Server

Windows Server és un sistema operatiu desenvolupat per Microsoft, dissenyat per a administrar xarxes, emmagatzematge i aplicacions a nivell empresarial. A diferència de les versions de Windows orientades a usuaris individuals, Windows Server està optimitzat per a la gestió de serveis en xarxa, com ara allotjament de llocs web, gestió de bases de dades i centralització de recursos compartits.

Algunes de les principals característiques de Windows Server inclouen:

- **Active Directory**: Un *servei de directori* que permet la gestió d’usuaris, equips i polítiques de seguretat en la xarxa de forma centralitzada.

- **Hyper-V**: Tecnologia de virtualització integrada que permet crear i administrar màquines virtuals.

- **IIS (Internet Information Services)**: Plataforma per a allotjar aplicacions web. Pemet que el servidor siga un servidor web.

- **Administració de servidors**: A través d’eines com l’Administrador de Servidors, es poden gestionar múltiples rols i funcions del servidor.

# 2. Instal·lació en un equip informàtic

Abans d’utilitzar Windows Server, és essencial entendre el procés d’instal·lació, que consta de diversos passos tècnics importants. Aquests asseguren que el sistema operatiu funcione correctament i que estiga ben integrat amb el maquinari i els recursos de la xarxa.

No obstant, no són molt diferents al ja vistos en un Windows 1x.

## 2.1 Particions i sistemes d’arxius

### Particions

Són divisions del disc dur on s’emmagatzemarà el sistema operatiu o les dades. Durant la instal·lació de Windows Server  (com la de qualsevol SO), cal triar com particionar el disc. 

### Taula de particions

Tenim dos possibilitats la més antiga **MBR (Master Boot Record)** i la més modern i hui en ia ja generalitzada **GPT**.

**MBR (Master Boot Record)**

- **Nombre màxim de particions:** 4 particions primàries.

Si es necessita més de 4 particions, es poden tenir 3 particions primàries i 1 partició estesa, dins la qual es poden crear múltiples particions lògiques.

- **Tipus de particions:**

  - **Primàries:** Particions directament accessibles pel sistema operatiu (màxim 4).

  - **Estesa:** Una partició que pot contenir particions lògiques.

  - **Lògiques:** Particions dins de la partició estesa.

- **Tamany màxim del disc**: 2 TB (terabytes), el MBR utilitza 32 bits per adreçar sectors de 512 Bytes.


### **GPT (GUID Partition Table)**
- **Nombre màxim de particions:** Teòricament il·limitat, però pràcticament uns 128 en la majoria de sistemes operatius.
- **Tipus de particions:**
  - Totes les particions són **primàries**, no hi ha limitació d'estesa o lògiques com en MBR.
- **Nombre màxim de particions:** Teòricament il·limitat, però pràcticament uns 128 en la majoria de sistemes operatius.
- **Tipus de particions:**
  - Totes les particions són **primàries**
- **Tamany màxim del disc** 9.4 ZB (zettabytes) teòrics.GPT utilitza un esquema d'adreces de 64 bits

### Sistema d’arxius.

Windows Server utilitza principalment el sistema **NTFS (New Technology File System)**, que ofereix millores respecte a FAT32. Les dos primeres superen les limitacions vistes al curs anterior de particions de 32GBytes i fitxers de 4GBytes de FAT32:

1. **Suport per a fitxers grans:** NTFS admet fitxers de fins a 16 TB, mentre que FAT32 està limitat a 4 GB per fitxer.
   
2. **Tamany màxim de partició:** NTFS suporta particions de fins a 256 TB, mentre que FAT32 es limita a 32 GB (en Windows).

3. **Seguretat i permisos:** NTFS permet assignar permisos d'accés més detallats que el simple Lectura/Escriptura/Control Total a fitxers i carpetes. 

4. **Encriptació (EFS - Encrypting File System):**. Permet xifrar fitxers i carpetes individualment, mantenint-lo protegit davant accés no autoritzat. ÉS una encriptació transparent per a l'usuari autoritzat que pot accedir-hi sense desxifrar manualment els fitxers, mentre que altres usuaris o sistemes no.
  
5. **Compressió (NTFS Compression):** Compressió nativa i transparent: el sfitxers i carpetes es poden utilitzar normalment sense necessitat de descomprimir-los manualment. Penalització molt poc el rendiment: molt útil si tenim fitxers que usem poc i més si tenen prou tamany.

6. **Recuperació d'errors:** NTFS registra les transaccions (journaling) i pot recuperar-se automàticament d'alguns errors del sistema, cosa que FAT32 no fa.

7 **Quotes de discs** Suposa poder limitar l'espai de disc per als usuaris. Ho practicarem en properes unitats.

8 **Fragmentació:** NTFS gestiona millor la fragmentació de dades en disc en comparació amb FAT32.

## Recomanacions generals

1. Usar GPT preferentment.
2. Separar en discos durs distints la instal·lació del SO de la resta de dades.
3. Si només tenim un disc (o conjunt que implementes una unitat tipus RAID), crearem particions distintes.
4. Si potser les crearem en el procés d'intal·lació o, almenys, abans de començar a operar. En cas contrari, hem de fer còpia de seguretat de les dades abans de reparticionar discos.
5. Si tenim discos de distint rendiment (SSD i HDD), és preferible que el SO i les aplicacions més utilitzades o de més demanda de capacitat de processament estiguen instal·lades al disc més ràpid.
6. Tant el backup com els sitems de redundància són fonamentals. (Els veiem més avant)

## 2.2 Gestors d’arrancada

El gestor d’arrancada és un programari que s’encarrega d’iniciar el sistema operatiu durant el procés d’arrancada de l’equip.

- **Windows Boot Manager** és el gestor d’arrancada predeterminat de Windows Server. Aquest programari s’encarrega de gestionar el procés d’arrancada del sistema i, en cas d’haver-hi diversos sistemes operatius instal·lats en l’equip, permet seleccionar quin iniciar.

## 2.3 Procés d’instal·lació

El procés d’instal·lació de Windows Server és similar al de les versions d’escriptori de Windows, però amb alguns passos addicionals per a la configuració de rols i característiques del servidor. Els passos generals inclouen:

1. **Preparar l’equip**: Comprovar els requisits de maquinari, com CPU, RAM i espai en disc.
2. **Arrancada des del mitjà d’instal·lació**: Açò pot ser des d’un DVD o una unitat USB bootable. Al VirtualBox usarem la unitat òptica.
3. **Selecció de la partició**: Triar la unitat o partició on s’instal·larà Windows Server.
4. **Configuració inicial**: Assignar un nom al servidor, configurar la xarxa i el compte d’administrador. Serà  administrador de tota la xarxa com vorem.
5. **Configuració de rols**: Durant o després de la instal·lació, es poden afegir rols al servidor. El més important és el de controlador de domini però hi ha altres.

Aquests darres aspectes el veiem de forma pràctica en al document següent.

# 3. Utilització de Windows Server

Una vegada instal·lat, és essencial familiaritzar-se amb la interfície i les funcionalitats bàsiques de Windows Server per a poder administrar-lo correctament.

## 3.1 Conceptes generals. Rols i característiques.

### Rols

Els rols són funcionalitats principals que pot exercir el servidor. En **Windows Server** hi ha diversos **rols de servidor** que poden ser instal·lats per oferir serveis específics a una xarxa o organització.

Rols principals de **Windows Server**:

1. **Active Directory Domain Services (AD DS)**: Permet crear i gestionar dominis, i és el component clau de l’Active Directory per a la gestió d'usuaris i dispositius en una xarxa.

2. **DHCP Server**: Assigna automàticament adreces IP als dispositius de la xarxa.

3. **DNS Server**: Traduïx noms de domini a adreces IP, facilitant l'accés als serveis dins d'una xarxa o a internet.

4. **File and Storage Services**: Gestiona el sistema d'emmagatzematge de fitxers i carpetes compartides, i permet utilitzar funcions com el servidor de fitxers, les quotes d'emmagatzematge i la deduplicació de dades.

5. **Remote Desktop Services (RDS)**: Proporciona eines per permetre que els usuaris es connecten de forma remota a escriptoris virtuals o aplicacions publicades.

6. **Print and Document Services**: Permet gestionar impressores i compartir-les en la xarxa.

7. **Hyper-V**: Proporciona funcions de virtualització per crear i gestionar màquines virtuals.

8. **Web Server (IIS)**: Hosteja aplicacions web i llocs web utilitzant **Internet Information Services (IIS)**.

9. **Network Policy and Access Services (NPAS)**: Permet la gestió de polítiques de xarxa i l'accés remot mitjançant serveis com el Servidor RADIUS.

10. **Windows Deployment Services (WDS)**: Serveix per desplegar de forma remota sistemes operatius als ordinadors clients a través de la xarxa.

11. **Active Directory Certificate Services (AD CS)**: Gestiona certificats digitals per a l'autenticació, la xifratge i la seguretat a la xarxa.

12. **Active Directory Federation Services (AD FS)**: Proporciona serveis d'inici de sessió únic (SSO) per a aplicacions.

13. **Active Directory Rights Management Services (AD RMS)**: Protegeix informació mitjançant polítiques de drets d'accés.

14. **Failover Clustering**: Proporciona alta disponibilitat a serveis i aplicacions mitjançant la creació de clústers de servidors.

15. **Windows Server Update Services (WSUS)**: Gestió de les actualitzacions de seguretat i programari per als clients de la xarxa.

16. **Host Guardian Service**: Ofereix protecció avançada per a entorns Hyper-V amb màquines virtuals protegides.

17. **Network Load Balancing (NLB)**: Permet distribuir el trànsit de xarxa entre múltiples servidors per assegurar alta disponibilitat i escalabilitat.

*En aquest curs ens centrerem en els 5 primers que són els fonamentals en qualsevol LAN*

### Característiques

A més dels rols, **Windows Server** ofereix **característiques** que són complements als rols i que proporcionen funcionalitats addicionals:

1.  **Windows PowerShell**. Llenguatge d'scripts basat em ordre (cmdLets) molt potent i avançat.

2.  **Servici de backup de Seguretat de Windows Server**. El vorem.

3.  **.NET Framework** (no el vorem)

4.  **Windows Defender**

5.  **Remote assistence**

Com veiem hi ha una clara relació entre el roll i la classificació dels servidors segons la funció que vam estudiar a la unitat anterior.

Per altra banda, la característica ve a complementar el roll o facilitar la tasca de l'administrador.

## 3.2 Llicències

Windows Server requereix llicències tant per al sistema operatiu com per als usuaris o dispositius que accedeixen al servidor (CALs - Client Access Licenses).

## 3.2 Interfície gràfica

La interfície gràfica de Windows Server és similar a la de les versions d’escriptori de Windows, però està optimitzada per a l’administració de servidors. Algunes característiques clau inclouen:

Al **curs de Windows 11** d'aquest mateix repositori podreu trobar informació sobre l'entorn gràfic comú de tots els Windows per a la gestió. Estudieu-ho.

[Introducció a l'entorn gràfic de Windows](https://tofermos.github.io/Windows11/interfaces/interfaces.html)

[Gestió des de l'entorn gràfic Windows](https://tofermos.github.io/Windows11/gestiodelequip/gestiodelequip.html)

### Sobre consoles i altres utilitats de gestió.

A bande de les vistes en l'apartat anterior i que són comunes, la pràctica totalitat, a tots els Windows tenim que,específicament de Windows Server les consoles i utilitats següents:


**servermanager.exe** - Administrador de Servidors. Aquesta és la utilitat (no es consola estrictament parlant) central per a gestionar el servidor. Permet configurar rols i característiques, gestionar discos, supervisar el rendiment, entre altres funcions.

**dcpromo.msc** - Promoció de controlador de domini: Utilitzada per configurar un controlador de domini (AD DS), una funció exclusiva de Windows Server.

**dnsmgmt.msc** - Gestió de DNS: Disponible en Windows Server per gestionar zones i registres DNS.

**dhcpmgmt.msc** - Gestió de DHCP: Permet administrar el rol de servidor DHCP per assignar adreces IP automàticament a dispositius de la xarxa.

**fsmgmt.msc** - Carpetes compartides: Una consola específica per gestionar carpetes i recursos compartits al servidor, encara que també es pot trobar en versions professionals de Windows 10/11.

**tsadmin.msc** o Remote Desktop Services Manager: Utilitzada per gestionar sessions d'escriptori remot, més comuna en Windows Server per administrar entorns d’escriptori remot (RDS).

**cluadmin.msc** - Gestió de Clúster de Failover: Disponible en Windows Server per administrar clústers de tolerància a fallades i alta disponibilitat, especialment útil per entorns crítics empresarials.

## 3.3 Tancament de sessió i apagat del servidor

El tancament de sessió i l’apagat d’un servidor requereix més precaucions que en una estació de treball (Windows 1x).

Respecte a l'**apagat del servidor**, aquets, solen estar executant serveis crítics i aplicacions de xarxa. Apagar incorrectament un servidor pot causar pèrdua de dades o interrupció del servei.
El procés d’apagat ha de ser planificat i queda registrada la causa que indiquem.

Respecte al **tancament de sessió**,  l'estat normal deu ser amb la sessió tancada excepte quan un administrador ha de fer alguna tasca. La sessió deu estar el temps estrictament necessari i el login no deu estar visible per raons de seguretat.

