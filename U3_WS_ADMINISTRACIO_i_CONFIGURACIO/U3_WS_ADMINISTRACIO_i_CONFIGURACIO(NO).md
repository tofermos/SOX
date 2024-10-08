---
title: U3. WINDOWS SERVER. ADMINISTRACIÓ I CONFIGURACIÓ 
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

# 1 Funcions d’un servidor

Des del punt de vista del que series les funcions d'un Sistema Operatiu de Xarxa trobem, com ja hem vist a l'anterior unitat, que es corresponen a alguns dels Rols i Característiques. Podem dir que Windows Server les implementa així. 
No tots els Rols i Característiques, menys encara, són funcions principals.

1. La funció de Servei de Directori que a Linux serà en OpenLDAP i vorem més avant, ací els el **Active Directory Domain Services (AD DS)**: Permet crear i gestionar dominis de forma centralitzada i còmoda.

2. La funció  o servei de **DHCP Server**: Assigna automàticament adreces IP als dispositius de la xarxa.

3. **DNS Server**: Traduïx noms de domini a adreces IP, facilitant l'accés als serveis dins d'una xarxa o a internet. Convé   que recordeu que quan cofiguràvem les IP en un WorkGroup NO indicàvem cap IP de servidor DNS. En canvi si buscàvem per la xarxa el nom del PC que compartia una carpeta, el trobàvem. En un Domini, usem la ressolució de noms, molt més eficient.

4. **File and Storage Services**: Gestiona el sistema d'emmagatzematge de fitxers i carpetes compartides, i permet utilitzar el servidor de fitxers, les quotes d'emmagatzematge i la deduplicació de dades.

5. La connexió remota pot considerar-se com una funció dels servidors. **Remote Desktop Services (RDS)** Proporciona eines per permetre que els usuaris es connecten de forma remota a escriptoris virtuals o aplicacions publicades.

6. **Print and Document Services**: Permet gestionar impressores i compartir-les en la xarxa.

7. **Web Server (IIS)**: Hosteja aplicacions web i llocs web utilitzant **Internet Information Services (IIS)**.

8.  **Servici de backup de Seguretat de Windows Server**. El vorem.

# 2 Administració i configuració bàsica

## Consoles i altres utilitats comuns a tots el sistemes Windows

Al curs de Windows 11 d'aquest repositori podreu trobar una guia més que suficient sobre les utitlitas gràfiques del sistema Windows per configurar i administrar una màquina.

[Consoles i altres utilitats](https://tofermos.github.io/Windows11/gestiodelequip/gestiodelequip.html)

## Consoles i altres utilitats específiques de Windows Server

A banda de les vistes en l'apartat anterior i que són comunes, la pràctica totalitat, a tots els Windows tenim que, específicament de Windows Server les consoles i utilitats següents:

**servermanager.exe** - Administrador de Servidors. Aquesta és la utilitat (no es consola estrictament parlant) central per a gestionar el servidor. Permet configurar rols i característiques, gestionar discos, supervisar el rendiment, entre altres funcions.

**dcpromo.msc** - Promoció de controlador de domini: Utilitzada per configurar un controlador de domini (AD DS), una funció exclusiva de Windows Server.

**dnsmgmt.msc** - Gestió de DNS: Disponible en Windows Server per gestionar zones i registres DNS.

**dhcpmgmt.msc** - Gestió de DHCP: Permet administrar el rol de servidor DHCP per assignar adreces IP automàticament a dispositius de la xarxa.

**fsmgmt.msc** - Carpetes compartides: Una consola específica per gestionar carpetes i recursos compartits al servidor, encara que també es pot trobar en versions professionals de Windows 10/11.

*(els 2 següents no anem a mirar-los en SOX)*

**tsadmin.msc** o Remote Desktop Services Manager: Utilitzada per gestionar sessions d'escriptori remot, més comuna en Windows Server per administrar entorns d’escriptori remot (RDS).

**cluadmin.msc** - Gestió de Clúster de Failover: Disponible en Windows Server per administrar clústers de tolerància a fallades i alta disponibilitat, especialment útil per entorns crítics empresarials.

## PowerShell (El vorem més avant)

Més avant, si farem una ullada interessant al lleguatge d'scripts basat en cmdLets (comandaments de Windows) molt avaçat i potent. 

Si voleu consultar, teniu un curs de PowerShell en aquest repositori:

[Curs PowerShell](https://github.com/tofermos/PowerShell)

# 3 Administració i configuració de comptes locals

Els comptes locals perden importància en un Domini. No obstant podeu consultar el curs de Windows 1x d'aquest repositori, ja que és un tema comú a tots els Windows Server.

[Comptes locals](https://tofermos.github.io/Windows11/gestions/comptesLocals.html)

# 4 Servei de directori

## 4.1 Conceptes de Directori i Domini

Bàsicament podem entendre un directori com una llista detallada d'objectes. Per
exemple, l'agenda de tlf del mòbil és un tipus de directori que guarda informació de contactes de persones o entitats. Objectes per igual (contactes) que poden tindre nom, adreça, tlf, e-mail, malnom...

### Dos classes d'informació

L'Active Directory Domain Services (AD DS) és molt semblant però més flexible i dinàmic. Permet emmagatzemar la informació
de tota l'organització. Una informació sobre l'estructura de l'propi directori i també sobre com s'ha d'administrar.
AD DS pot emmagatzemar informació sobre l'organització, llocs, ordinadors, usuaris, objectes compartits i qualsevol altra cosa que pugui formar part de la infraestructura de xarxa. A diferència de la llista de contactes de whatsapp, ací els elements emmagatzemats son de diferent naturales (usuaris, grups, UO, polítiques d'accés, permisos, assignació de recursos,
etc.)
Cal recordar que tota aquesta informació s'emmagatzema en una base de dades jeràrquica.

### Replicable

El motor d'aquesta base de dades és el mateix que incorpora Microsoft Exchange Server i
permet la replicació de controladors de domini. És a dir, es pot enviar la informació continguda a la
base de dades a diferents controladors de domini a través de la xarxa. 

A més d'administrar polítiques que seran vàlides a tota l'organització, Active Directory permet
realitzar operacions com la instal·lació de programes, de manera simultània i centralitzada, en
multitud de clients o aplicar actualitzacions crítiques en tota l'organització.

Quan utilitzem Active Directory, tenim a la nostra disposició eines d'administració per establir
polítiques de grup, per incloure uns grups dins d'altres en diferents nivells, un accés senzill a l'arbre
d'usuaris, ordinadors, impressores i contactes, etc. Òbviament, podem utilitzar Windows Server
2019 sense usar Active Directory, però estarem prescindint d'un ampli conjunt de capacitats.

### Dos tipus de components

Pel que fa a l'estructura de l'servei de directori, el primer que hem de saber és que hi ha dos tipus de
components en Active Directory: els components físics i els components lògics.

*Taula 1: Components de l'AD-DS*

|Components físics|Components lògics|
|:---|:--|
|Controladors de dominis|Dominis|
|Llocs|Boscos|
|Xarxes|Arbres|
||Unitats organitzatives|

>Nota:
>
>Al mòdul de SOX ens centrem en desenvolupar un model senzill d'organització: un Domini amb Unitats Organitzatives.

## 4.2 Instal·lació del Active Directory 

Teniu una guia molt resumida en el curs de Windows Server d'aquest repositori. Entreu al següent enllaç...

[Instal·lació del AD](https://github.com/tofermos/Windows-Server/blob/main/md/ADDSenWindowsServerGUI.md)

## 4.3 Usuaris del Domini

A la present unitat i en avant, anem a centrar-nos en els usuaris del domini. Sobre usuaris locals (els que usem en monoestació o WorkGroup) teniu tota la informació al curs de Windows 1x d'aquest repositori.

Recordem que els grups són un tipus de contenidor que permeten definir conjunts d'usuaris i definir permisos basant-nos en aquesta pertinença al grup, en lloc de fer-ho de manera individual, usuari per usuari.
Com a pauta general, l'agrupació d’objectes sol facilitar les tasques d’administració reduint les possibilitats d’error.

### Creació d'usuaris

Tot i que després vorem com poden ser els usuaris, és a dir a quin o quins grups poden pertànyer, fem una mirada prèvia al manteniment dels usuaris per donar un enfoc pràctic i més dinàmic.

### Des de l'Adminsitrador de l'Active Directory

![](png/usuaris1.png)

### Creem un usuari

![](png/usuaris2.png)

![](png/usuaris3.png)

![](png/usuaris4.png)

### Configurem el compte d'usuari creat

![](png/usuaris5.png)

![](png/usuaris6.png)

![](png/usuaris7.png)

![](png/usuaris8.png)

![](png/usuaris9.png)

## 4.4 Grups d'usuaris en l'AD

### Tipus i àmbits

Hi ha dos grans tipus de grups al Directori Actiu del Windows:

**Grups de seguretat:** aquest tipus de grups permet definir permisos per a recursos del domini. Són els utilitzats a les llistes de control d'accessos (ACLs) que s'estudiaran més endavant. Aquest tipus de grups són els que s'utilitzaran a la administració de la xarxa.

**Grups de distribució:** no tenen característiques de seguretat, únicament són un llistat d'usuaris per a missatgeria.

Dins dels grups de seguretat hi ha tres àmbits:

**Grup Universal:** és un grup els permisos del qual s'estenen a diversos dominis. A més, aquest tipus de grups pot estar format per usuaris o grups d’usuaris de diferents dominis.

**Grup Global:** és molt similar als grups universals, és a dir poden permetre l'accés a recursos de qualsevol dels dominis de l'arbre del Directori Actiu, però llevat que tots els membres del grup deuen pertànyer al mateix domini.

**Grup local del domini:** és un grup creat en un domini amb membres que poden provenir d'altres dominis i que només pot tenir accés a recursos dins del domini.

**En quins casos utilitzarem cada àmbit?** 

Els grups universals solen tenir la seva utilitat en grans empreses on s'ha definit un bosc de dominis
assignant dominis a cadascun dels seus departaments o divisions. En aquest tipus d'estructures, quan se'n realitza una modificació en el grup, aquesta ha de replicar-se en tots els controladors de domini que estiguin configurats com a catàleg global.
En xarxes de domini únic es poden aplicar grups globals que tindran més sentit quan es defineixi un segon domini, el que pot passar en el moment en què hi hagi una ampliació de l’organització.

Com a pautes generals per a l'administració de xarxes tindrem en compte les consideracions següents

1. No cal assignar un àmbit més ampli del necessari.

2. Els grups locals de domini no es poden processar a altres dominis.

3. Un grup global no es replica fora del domini, ja que no forma part del pla de replicació del catàleg global.

4. Els grups universals es repliquen per tota la xarxa generant trànsit que tenia certa incidència en el rendiment abans dels Windows Server 2008. hui en dia en té poca.

5. Si un grup universal està compost per grups globals i es produeixen canvis dins dels grups globals, no es produeix un canvi al catàleg global, i per tant aquesta modificació no comporta una replicació en tots els controladors de domini del bosc.

### Grups predefinits

En instal·lar el Directori Actiu podem comprovar que s'han generat automàticament una sèrie de grups predefinits amb uns permisos d'acord amb les funcions assignades:

![](png/usuaris10.png)

Examinem les funcions d'alguns dels grups més utilitzats:

**Usuaris del domini:** grup global que conté tots els comptes d'usuaris del domini.

**Administradors del domini:** grup global que permet als membres realitzar tasques d'administració del domini.

**Administradors d'empresa:** grup universal que permet als membres realitzar tasques d'administració a tots els dominis de la xarxa.

**Administradors d'esquema:** grup universal que permet als membres modificar l'estructura dels objectes del Directori actiu.

**Administradors:** grup local que permet als seus membres realitzar tasques d'administració al controlador de domini.
Operadors de còpies de seguretat: grup local que permet als seus membres fer còpies de seguretat o restaurar fitxers dins del domini.

**Operadors de compte:** grup local que permet als membres crear, editar i eliminar comptes d'usuari i grups.

**Operadors d'impressió:** grup local que permet als membres configurar i administrar l'ús d'impressores de xarxa.

**Operadors de servidor:** grup local que permet als seus membres crear carpetes compartides al servidor i realitzar còpies de seguretat o restaurar fitxers al controlador de domini.

**Usuaris:** grup local que limita les possibilitats que un usuari faci un canvi accidental al sistema però sí permet executar la majoria de les aplicacions.

### Creació de grups.

![](png/usuaris11.png)

![](png/usuaris12.png)

### Com afegir usuaris al grup.

Opció 1: Propietats del grup...

![](png/usuaris13.png)

Opció 2: Des de les Propietats de l’usuari...

![](png/usuaris14.png)

![](png/usuaris15.png)

## 4.5 Unitats organitzatives

Teniu una guia resumida en el curs de Windows Server d’aquest repositori. Entreu al següent enllaç…

[Curs Windows Server. Unitats Organitzatives](https://github.com/tofermos/Windows-Server/blob/main/md/UnitºatsOrganitzatives.md)

# 5 Servei DNS

L'objecte del mòdul no és l'estudi dels serveis sinó dels Sistemes Operatius. En aquest cas la integració del servei DNS amb el Windows Server.

Aquests punt s'aboradarà des de 3 punts de vista:

* Un enfoc teòric en aquest apartat
* Vorem, més avant, alguns cmdLets per instal·lar/desinstal·lar, consultar i fer algun canvi.

## 5.1 La integració del DNS al servei AD DS

El servei de servidor DNS està integrat en el disseny i implementació dels serveis de domini d'Active Directory (AD DS), proporcionant una eina empresarial per organitzar, gestionar i localitzar recursos en una xarxa.

Quan implementeu servidors DNS amb AD DS, tingueu en compte que:

- El DNS és necessari per localitzar els controladors de domini.
- El servei d'inici de sessió a la xarxa utilitza el servidor DNS per registrar els controladors de domini al vostre espai de noms DNS.
- Els servidors DNS amb Windows Server poden utilitzar AD DS per emmagatzemar i replicar les zones DNS.
- La integració de zones DNS amb AD DS permet funcions com la rèplica d'AD DS, actualitzacions dinàmiques segures, i l'envelliment i eliminació de registres.

**Com s'integra DNS amb AD DS**

Quan instal·leu AD DS en un servidor i el promocioneu a controlador de domini, se us demana que especifiqueu un nom de domini DNS per al domini AD DS. A més, se us ofereix l'opció d'instal·lar el servidor DNS, ja que és necessari per localitzar controladors de domini dins del domini AD DS.

## 5.2 Beneficis de la integració d'AD DS

Per a xarxes que utilitzen DNS per a AD DS, es recomana utilitzar zones primàries integrades al directori, ja que aporten diversos beneficis:

- **Replicació multimaster**: Amb AD DS, qualsevol servidor DNS pot acceptar actualitzacions dinàmiques i replicar-les entre tots els servidors DNS.
- **Seguretat millorada**: Mitjançant ACLs, es poden restringir les actualitzacions dinàmiques per a equips o grups específics, cosa que no és possible amb zones primàries estàndard.
- **Automatització i sincronització**: Quan es crea un nou controlador de domini, les zones es repliquen automàticament.
- **Millor rendiment**: La sincronització de les zones integrades al directori és més eficient que les actualitzacions estàndard, evitant la transferència de tota la zona.

Si integreu les zones DNS amb AD DS, també simplifiqueu la gestió de la rèplica de bases de dades, evitant la necessitat de mantenir topologies de rèplica separades per a DNS i AD DS. Aquesta integració permet visualitzar la gestió com una única entitat administrativa.

Finalment, només les zones primàries es poden emmagatzemar al directori. Les zones secundàries han d'emmagatzemar-se en fitxers de text estàndard, però amb el model de replicació multimaster d'AD DS, no són necessàries si totes les zones estan en AD DS.

# 6 Servei de DHCP

El servei **DHCP (Dynamic Host Configuration Protocol)** en **Windows Server** és una funció que permet als administradors de xarxa automatitzar l'assignació d'adreces IP i altres paràmetres de configuració de xarxa als dispositius que es connecten a la xarxa.

## 6.1 Funcionament del servei DHCP

Es tracta d'un típic servici que respon a la filosofia del model client servidor. Quan un dispositiu (com un ordinador, càmera IP, mòbil, impressora...) es connecta a la xarxa, envia una sol·licitud per obtenir una adreça IP. El servidor DHCP respon a aquesta petició assignant-li una adreça IP de manera automàtica i dinàmica, així com altres paràmetres de configuració de xarxa com:

- **Adreça IP**: Una adreça única dins del rang establit pel servidor.
- **Màscara de subxarxa**: Indica la porció de la xarxa a la qual pertany l'adreça IP.
- **Passarel·la predeterminada**: Normalment, és l'adreça del router o un altre dispositiu de xarxa que connecta la xarxa local amb Internet.
- **Servidors DNS**: Les adreces dels servidors que resolen els noms de domini a adreces IP.

## 6.2 Avantatges del servei DHCP en Windows Server

- **Gestió centralitzada**: DHCP facilita la gestió de les adreces IP des d'un servidor central, evitant la configuració manual de cada dispositiu.
- **Eficàcia**: Assegura que no es produeixin conflictes d'adreces IP duplicades a la xarxa.
- **Escalabilitat**: És especialment útil en xarxes grans, on assignar IPs manualment seria lent i poc pràctic. 
- **Flexibilitat**: Si volem un canvi de totes les IP o gran part, només hem de configurar-lo al servici i reiniciar el dispositius. Imaginem, per exmeple, passar de IPv4 de classe C a B per a tota una xarxa.
- **Actualitzacions automàtiques**: El servidor DHCP pot canviar les adreces IP dels dispositius a mesura que es connecten i desconnecten de la xarxa.
- **Concessió temporal d'adreces IP**: Les IPs es poden assignar amb una duració específica, de manera que quan un dispositiu deixa de ser necessari a la xarxa, l'IP es pot reutilitzar.

### Components principals del DHCP

- **Rangs o àmbits**: Un conjunt de configuracions que defineixen un rang d'adreces IP que es poden assignar als dispositius clients.

- **Exclusions**: Quan volem que dins del rang alguna IP o grup d'IPs ("subrangs") no s'assignen. Pot ser útil per si volem assignar-les de forma fixa a determinats dipositius.

- **Reserves**: Permeten assignar una IP fixa a un dispositiu en particular basat en la seva adreça MAC, assegurant que sempre obtinga la mateixa IP.

- **Opcions DHCP**: Paràmetres addicionals, com ara passarel·les (router o gateway) predeterminades o DNS, que el servidor DHCP pot proporcionar als dispositius clients.

### Funcionament del procés DHCP

1. **Discover**: El client envia una petició en difusió per trobar un servidor DHCP a la xarxa.
2. **Offer**: El servidor DHCP respon oferint una adreça IP.
3. **Request**: El client accepta l'oferta enviant una sol·licitud per a l'adreça IP.
4. **Acknowledge**: El servidor DHCP confirma l'assignació de l'adreça IP al client.

![*Figura1: Esquema C/S*](png/DHCPesquema.jpg)

En resum, el servei DHCP en Windows Server facilita la gestió i assignació automàtica d'adreces IP en una xarxa, millorant l'eficiència i reduint la complexitat de la configuració manual de xarxes.

## 6.3 Enfoc pràctic

Aquests punt s'aboradarà des de 3 punts de vista:

* Un enfoc teòric.
* Vorem, més avant, alguns cmdLets per instal·lar/desinstal·lar, consultar i fer algun canvi.
* Un enfoc pràctic en usar-los en les activitats desenvolupades des del GUI que abordem al següent apartat mitjançant el curs de Windows Server d'aquest repositori.

# 6.4 DHCP. Implementación


Teniu una guia molt resumida en el curs de Windows Server d'aquest repositori. Entreu al següent enllaç...

[](https://github.com/tofermos/Windows-Server/blob/main/md/DHCP.md)




