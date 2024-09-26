---
title: U3. WINDOWS SERVER. ADMINISTRACIÓ I CONFIGURACIÓ (IV)
subtitle: SERVEI DHCP
author: "@tofermos 2024"
lang: "ca-ES"
papersize: A4
linestretch: 1.5
output:
  pdf_document:
    toc: true
    keep_tex: true
    latex_engine: xelatex
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
    df_print: paged
    number_sections: false
---

\newpage
\renewcommand\tablename{Tabla}

# 1 Servei de DHCP

Es tracta de la funció bàsica de proporcionar IP provades en les LAN . Encara que aquest servei en altres tipus de xarxes el donen diferents dispositius com és el cas de les WIFIS públiques o el router domèstic.


## 1.1 Conceptes previs

```
Recordatori:  Conceptes del mòdul de XAL de 1r SMX

- Adreça IP: Una adreça única dins del rang establit pel servidor.

- Màscara de subxarxa: Indica la porció de la xarxa a la qual pertany l'adreça IP.

- Adreça MAC: Adreça única de cada dispositiu (tarja ethernet, WIFI o boca d'un switch). Conté 6 bytes, els 3 més alts identifiquen el fabricant. Els altres 3, al dipositiu de forma única.

- Passarel·la predeterminada: Normalment, és l'adreça del router o un altre dispositiu de xarxa que connecta la xarxa local amb Internet.

-Servidors DNS: Les adreces dels servidors que resolen els noms de domini a adreces IP.

```

El servei **DHCP (Dynamic Host Configuration Protocol)** en **Windows Server** és una **funció de servidor** que permet als administradors de xarxa automatitzar l'assignació d'adreces IP i altres paràmetres de configuració de xarxa als dispositius que es connecten a la xarxa.

## 1.2 Funcionament del servei DHCP

Aquesta funció s'implementa en Windows Server com un servici que respon a la filosofia del model client servidor. 
Quan un dispositiu (com un ordinador, càmera IP, mòbil, impressora...) es connecta a la xarxa, envia una sol·licitud per obtenir una adreça IP. 
El servidor DHCP respon a aquesta petició assignant-li una adreça IP de manera automàtica i dinàmica, així com altres paràmetres de configuració de xarxa com:

1. **Discover**: El client envia una petició en difusió per trobar un servidor DHCP a la xarxa.
2. **Offer**: El servidor DHCP respon oferint una adreça IP.
3. **Request**: El client accepta l'oferta enviant una sol·licitud per a l'adreça IP.
4. **Acknowledge**: El servidor DHCP confirma l'assignació de l'adreça IP al client.

![*Figura1: Esquema C/S*](png/DHCPesquema.jpg){width=60%}


## 1.3 Elements principals del DHCP

- **Rangs o àmbits**: Conjunt o rang d'adreces IP que es poden assignar als dispositius clients.

- **Exclusions**: Quan volem que dins del rang alguna IP o grup d'IPs ("subrangs") no s'assignen. Pot ser útil per si volem assignar-les de forma manual a determinats dipositius.

- **Reserves**: IP que el DHCP assigna sempre al matei dispositiu. Es basa en la seua MAC.

- **Opcions DHCP**: Paràmetres addicionals, com ara passarel·les (router o gateway) predeterminades o DNS, que el servidor DHCP pot proporcionar als dispositius clients.


En resum, el servei DHCP en Windows Server facilita la gestió i assignació automàtica d'adreces IP en una xarxa, millorant l'eficiència i reduint la complexitat de la configuració manual de xarxes.


# 2 Implementació en Windows Server 2019

Veiem com s'implementa aquesta funció típica d'un model Client/Servidor el Windwos Server 2019.

## 2.1 Instal·lació

Un dels servicis més típics i usats d'un servidor senzill de xarxa és l'assignació dinàmica de IP privades de la xarxa local. Veiem com instal·lem i configurem el servei.

![](png/DHCP1.png){width=60%}

![](png/DHCP2.png){width=60%}

![](png/DHCP3.png){width=60%}

![](png/DHCP4.png){width=60%}

![](png/DHCP5.png){width=60%}

![](png/DHCP6.png){width=60%}

## 2.2 Configuració

![](png/DHCP7.png){width=60%}

### Creem un rang de IP

**Pasos recomanables** 
* El/s rang/s ha d'abarcar TOTES les volem a la xarxa local. Després indicarem (dins de rang) quines queden excloses:
* Exclourem les que vulguem assignar manualment (estàtiques)... ( servidors, altres routers...). Vorem que el servidor DNS i el Gateway (encaminador) ens el demana ara.
* Exclourem les que posem a disposició d'altri (administrador extern de fotocopiadores de xarxa o de càmeres IP, per exemple).
* Fer les reserves de IP per a determinades MAC: Assignem les IP ( dinàmica) però constant a un dispositiu en concret
  
![](png/DHCP8.png){width=60%}

### Exclusions

![](png/DHCP9.png){width=60%}

### Duració de l'assignació

![](png/DHCP10.png){width=60%}


![](png/DHCP11.png){width=60%}

### Gateway (porta d'enllaç) o encaminador

Indiquem la IP que hem configurat al router.

![](png/DHCP12.png){width=60%}

### Servidor DNS

Indiquem la IP que té el servidor amb el servei DNS.

![](png/DHCP13.png){width=60%}

# 3 DHCP com servei

Mirarem més avant els servicis però podem observar ja alguna característica típica d'este software de servidor. Entrem a la consola de Serveis (**services.msc**):

* Podem reiniciar-lo o aturar-lo
  
![](png/ServiciDHCP0.png){width=60%}

* El tipus d'inici per defecte és, obviament, automàtic

 ![](png/ServiciDHCP1.png){width=60%}

* No està associat a cap compte d'usuari en concret que haja d'iniciar sessió

  Només cal iniciar sessió en un servidor puntualment per a tasques de manteniment.
  
![](png/ServiciDHCP2.png){width=60%}


# 4 Client 

En el client devem canviar la configuració de la NIC i especificar que, ara, la IP l'assignarà el DHCP

![](png/DHCP14.png){width=60%}

## 4.1 El servici DHCP en el client.

* L'assignació dinàmica de IP o protocol DHCP respon al **model client servidor**.
* Altra qüestió és que la implementació del "client DHCP" en els PC satèlits o clients siga mitjançant un "servei local de Windows".
* Per vore el software client, entrem en la consola de serveis: **services.msc**

|Model C/S| Nom del servici Windows|Acció|
|---|---|---|
|Servidor DHCP|"Servicio DHCP" en Windows Server|Reb peticions de IP de dispositius i les assigna|
|Client DHCP|"Cliente DHCP" en Windows 1x|Sol·licita IP i, en rebre-la, la configura a la tarja|

![](png/ServiciClientDHCP0.png){width=60%}
![](png/ServiciClientDHCP1.png){width=60%}
![](png/ServiciClientDHCP2.png){width=60%}

# 5 Avantatges del servei DHCP en Windows Server

- **Gestió centralitzada**: DHCP facilita la gestió de les adreces IP des d'un servidor central, evitant la configuració manual de cada dispositiu.
- **Eficàcia**: Assegura que no es produeixin conflictes d'adreces IP duplicades a la xarxa.
- **Escalabilitat**: És especialment útil en xarxes grans, on assignar IPs manualment seria lent i poc pràctic. 
- **Flexibilitat**: Si volem un canvi de totes les IP o gran part, només hem de configurar-lo al servici i reiniciar el dispositius. Imaginem, per exmeple, passar de IPv4 de classe C a B per a tota una xarxa.
- **Actualitzacions automàtiques**: El servidor DHCP pot canviar les adreces IP dels dispositius a mesura que es connecten i desconnecten de la xarxa.
- **Concessió temporal d'adreces IP**: Les IPs es poden assignar amb una duració específica, de manera que quan un dispositiu deixa de ser necessari a la xarxa, l'IP es pot reutilitzar.



