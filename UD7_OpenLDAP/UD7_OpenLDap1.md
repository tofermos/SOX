---
title: |
  | U7-OpenLDAP 
subtitle: | 
  | Introducció i instal·lació
author: "Tomàs Ferrandis Moscardó"
output:
  html_document:
    toc: false
    toc_float: true
    toc_depth: 2
    df_print: paged
    number_sections: false
  pdf_document: 
    toc: false
    keep_tex: true
  word_document:
    toc: true
    toc_depth: '3'
lang: "es-ES"
fontsize: 12 pt
mainfont: Times New Roman
papersize: A4
linestretch: 1.5
---

# 1  LDAP. Introducció

LDAP significa Lightweight Directory Access Protocol. Com el seu nom indica, és un protocol lleuger en mode client-servidor per accedir als serveis de directori. És adir per a emmagatzemar i gestionar informació sobre organitzacions, usuaris, xarxes, etc. S'executa sobre TCP/IP o altres protocols orientats a connexió.

Ja hem vist en unitats anteriors ben bé què és un directori (Active Directoy de Windows Server)
 	 	
## 1.1  Com funciona LDAP?

Età basat en un model client-servidor. Un client LDAP es connecta a un servidor LDAP i li fa una consulta. El servidor respon ( o apuntaria a un altre servidor). Dèiem abans que poden haver molts servidors amb les dades replicades: per tant no és problema que un client es connecte a un o a un altre; veurà sempre la mateixa vista del directori.	

## 1.2  Avantatges en l'ús de LDAP

Un directori LDAP destaca sobre els altres tipus de bases de dades per les següents característiques:

• És molt ràpid en la lectura de registres.
• Permet  replicar el servidor de forma molt senzilla i econòmica.
• Moltes aplicacions de tot tipus tenen interfícies de connexió a LDAP i es poden integrar fàcilment.
• Disposa d'un model de noms globals que assegura que totes les entrades són úniques.
• Utilitza un sistema jeràrquic d'emmagatzematge d'informació.
• Permet múltiples directoris independents
• Funciona sobre TCP/IP i SSL
• La majoria de servidors LDAP són fàcils d'instal·lar, mantenir i optimitzar. 	

## 1.2.1  Usos pràctics de LDAP

• Directoris d'informació. Per exemple bases de dades d'empleats organitzats per departaments (seguint l'estructura organitzativa de l'empresa) o qualsevol tipus de pàgines grogues.
• Sistemes d'autenticació / autorització centralitzada. Grans sistemes on es guarda gran quantitat de registres i es requereix un ús constant dels mateixos. Per exemple: Active Directory Server de Microsoft, per gestionar tots els comptes d'accés a una xarxa corporativa i mantenir centralitzada la gestió de l'accés als recursos.
• Sistemes d'autenticació per a pàgines web, alguns dels gestors de continguts més coneguts disposen de sistemes d'autenticació a través de LDAP. 	
• Sistemes de control d'entrades a edificis, oficines ....
• Sistemes de correu electrònic. Grans sistemes formats per més d'un servidor que accedeixin a un repositori de dades comú.
• Sistemes d'allotjament de pàgines web i FTP, amb el repositori de dades d'usuari compartit.
• Grans sistemes d'autenticació basats en RADIUS, per al control d'accessos dels usuaris a una xarxa de connexió o ISP.  
• Servidors de certificats públics i claus de seguretat.
• Autenticació única o "single sign-on" per a la personalització d'aplicacions.
• Perfils d'usuaris centralitzats, per permetre itinerància o "Roaming"
• Llibretes d'adreces compartides.

Alguns exemples

Sistema de correu electrònic
Cada usuari s'identifica per la seva adreça de correu electrònic, els atributs que es guarden de cada usuari són la seva contrasenya, el seu límit d'emmagatzematge (quota), la ruta del disc dur on s'emmagatzemen els missatges (bústia) i possiblement atributs addicionals per activar sistemes anti-spam o antivirus.

Com es pot veure aquest sistema LDAP rebrà centenars de consultes cada dia (una per cada correu electrònic rebut i una cada vegada que l'usuari es connecta mitjançant POP3 o webmail). No obstant el nombre de modificacions diàries és molt baix, ja que només es pot canviar la contrasenya o donar de baixa a l'usuari, operacions ambdues que no es realitzen de forma freqüent.

Sistema d'autenticació a una xarxa
Cada usuari s'identifica per un nom d'usuari i els atributs assignats són la contrasenya, els permisos d'accés, els grups de treball als quals pertany, la data de caducitat de la contrasenya, etc...

Aquest sistema rebrà una consulta cada vegada que l'usuari accedeixi a la xarxa i una més cada vegada que accedeixi als recursos del grup de treball (directoris compartits, impressores ...) per comprovar els permisos de l'usuari.
Enfront d'aquests centenars de consultes només unes poques vegades es canvia la contrasenya d'un usuari o se l'inclou en un nou grup de treball.


