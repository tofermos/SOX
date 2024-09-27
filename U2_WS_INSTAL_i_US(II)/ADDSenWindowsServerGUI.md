---
title: U2. Windows Server. Instal·lació i ús (II)
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

# 1 Resum

En aquesta unitat prèvia a la creació d'un domini:

1. Configurarem les MV com a Xarxa Interna. Emulem una xarxa local de computadores connectades a un switch.

2. Configurarem la xarxa Windows mitjançant IP fixes privades en la mateixa xarxa. 

3. Coneixerem sobre protocols i Firewall :

  * protocols i aplicacions (detecció de xarxes i compartició en Windows) 
  * altres prrotocols com el ICMP4 (ping) o SMB
  * les restriccions del Firewall
  
4. Estudiarem aspectes bàsics de la compartició de carpetes.

5. Treballarem la captura d'unitats (GUI/CLI)

6. Veurem algunes configuracions generals simples ( nom del PC i WG, Actualizacions automàtiques, Zona horària...)

# 2 Canviar el nom del servidor i Workgroup


![*Figura1: Nom equip i del Grup de Treball*](png/ADDS/WorkgroupNomEquip.png)


Configurar la xarxa Servidor


# 3 Configuració de la xarxa en Virtualbox. "Xarxa Interna"

Estem "conectant cables al switch".

De moment només ens fa falta la tarja que es connectarà a un switch on es conecten la  resta de PC de la xarxa () "xarxa interna").

Podem instal·lar un segon adaptador per disposar de la connexió d'Internet de l'amfitrió (adaptador NAT). De moment és opcional.

![*Figura 2: Xarxa interna*](png/ADDS/xarxainterna.png)

> NOTA:
>
> En el WINDOWS 1x hem de tindre NOMÉS la tarja interna. No perdeu de vista la "realitat" que estem emulant !

# 4 Configuració de la xarxa en Windows.

## 4.1 Firewall de Windows. Aplicaciones permitidas

Des del mateix Administrador de Servidor accedir al **Firewall: Aplicaciones permitidas** i assegurar que ens permeta Compartir i Detectar recursos a través de la xarxa. Dos capacitat que activarem en l'apartat següent:

![*Figura 3: Permitir una aplicación a través de Firewall*](png/ADDS/PermitirUnaAplicacionATravesdeFirewall.png)


![*Figura 4: Firewall permet compartir y detectar xarxes*](png/ADDS/FirewallCompartiryDetectar.png)

`

## 4.2 IPs privades en la mateixa xarxa

Com ja sabeu del mòdul de XAL de 1r de SMX haureu de configurar les IPs. Per exemple:

   IP Windows 1X: 192.168.0.2/24    
   IP Windows Server: 192.168.0.1/24
    
*Windows+R: Configuración, Red e internet, Centro de Redes y Recursos Compartidos, Ethernet*

![*Figura 5: Adaptador de xarxa*](png/ADDS/tarja2.png)


## 4.3 Detecció de xarxes i recursos


*Windows+R: Configuración, Red e internet, Centro de Redes y Recursos Compartidos*

Com ja vam estudiar a la Unitat anterior amb el Wordgroup fet amb PC Windows 1x, hem d'activar per a las ***xarxa  privada** en totes les màquines

* Activar la detecció de xarxes

* Activar l'ús compartit de carpetes i impressores.
 
Configuración, Red e internet ( o *Win + I*) Centro de Redes y Recursos Compartidos, Cambiar configuración del  Uso compartido avanzado:*
    
![](png/ADDS/ActivaDetecciónyUsoCompartidoPrivada.png)

![](png/ADDS/ActivaDetecciónyUsoCompartidoPublico.png)

![](png/ADDS/ActivaUsoCompartidoCarpetasPúblicasyContrasenya.png)

## 4.4 Problema en Windows Server i la Xarxa Privada

**Problema:**

En Xarxa Privada, marquem les opcions però quan entrem veiem que estan desactivades 
No es detecten les carpetes compartides, ni tant sols els PCs de la xarxa.

En canvi sí podem accedir a les carpetes mitjançant els comandament *net use*

**Raó**:

La detecció de serveis compartits depén d'altres serveis que no estan executant-se. 

**Solució:**

Abans que res assegureu-vos que teniu el Firewall configurat com hem indicat al punt anterior (**Aplicaciones permitidas...**). Si és correcte...

Fent spoiler al tema de **Serveis de Windows** que tractarem més avant, cal que activem una sèrie de serveis necessaris (dependències)

Alguns d'aquests servicis podrem inciar-los des l'Administrador del Servidor (*servermanager.exe*) que tenim obert normalment però altres no. 
Això es deu a que no estan habilitats, caldrà executar la consola de microsoft específica de servicis (*services.msc*) i habilitar-los prèviament.

Els serveis que cal que estiguen executant-se (dependències) són:

* Client DNS
* Publicación de resursos de deteccción de 
* Detección host de SSDP
* Dispositivo host de UPnP


Per a iniciar els servicis primer cal que estiguen habilitats. Per això anem a la **consola MC de servicis** amb *Win R: services.msc*).

1.- Assegurem que estiguen no estiguen deshabilitats. El tipus d'inici ha de ser **Automático** per a que s'engeguen en iniciar el servidor.

![*Figura 8: Canviem en tipus d'inici a Automático*](png/ADDS/ServicioDetecciónSSDP.png)


2.- Podem iniciar manualment per no reiniciar ara.

![*Figura 9: Iniciem manuelament un servici*](png/ADDS/ServiciInici.png)


>Nota:
> 
>Encara que supose continuar fent spoiler sobre el tema de servicis, observa que amb un **Inici automàtic**, el servei està en marxa en engegar-se el servidor **sense necessitat d'iniciar sessió al Servidor**
## 4.5 Provar la connectivitat amb el protocol ICMP (ping)

Una prova molt clàssica és la del ping (protocol ICMP4). La fem des de totes les màquines.

![*Ping des de totes les màquines de la xarxa*](png/ADDS/ping.png)

Si tenim problemes podem revisar, la configuració del Firewall:


!*Firewall ICMP4 (echo entrada)*](png/ADDS/FirewallICMP4Entrada.png)

![*Firewall ICMP4 (echo salida)*](png/ADDS/FirewallICMP4Salida.png)



# 5 Aspectes bàsics de la configuració des del *msconfig*


Un exemple podria ser desactivar/activar el **Servei d'actualitzacions**

> Nota sobre les actualitzacoins automàtiques
>
> És important que entengueu el que pot suposar tindre activada esta opció en un servidor real aplicacions i middleware instal·lat i molts clients depenent-ne.

*Win + R: msconfig.exe*

<img width=60% src="png/ADDS/configurarsistema.png"></img>


Altre exemple podria ser assegurar la **Zona horària**.

Cal connexió a Internet. Caldrà una segona tarja connectada a un router (NAT en l'emulació nostra de Virtualbox)

<img width=60% src="png/ADDS/zonahoraria.png"></img>

# 6 Recursos compartits en xarxa


## 6.1 Compartició de carpetes

La compartició de carpetes la farem sense especificar permisos per a usuaris donat que encra no tenim ususari del domini. No anem a "replicar-los" com hem fet en un Workgroup. 

![*Compartició de carpeta*](png/ADDS/Compartir.png)

Pondem limitar el nombre d'usuaris que hi poden accedir

![*Compartició de carpeta*](png/ADDS/Compartir2.png)

>Nota:
>
> La limitació d'usuaris és important per raons de seguretat (evitar accesos desconeguts) però també per a mateniment: controlar si es queden sessions sense tancar . Des de la consola del sistema de fitxers es podrien expulsar.
D'igual manera passaria amb els fitxers oberts.


## 6.2 Assignació o captura d'Unitat de Xarxa

Ja ho hem vist anteriorment amb el Net use, però una vegaga funciona correctament la els protocols que faciliten *la compartició de carpetes i impressores* i la *detecció de la xarxa*, podem assignar unitats a través del GUI buscant el recurs per la xarxa.
Simplement amb botó contrari *Asignar unidad de red*. En reiniciar el client vorem que continua (el mateix efecte que el /persistent:yes).

La forma en que es podrà automatitzar esta captura per a tots els clients d'una xarxa la vorem més avant.

## 6.3 Net use

Amb els comandaments *net* podem, entre d'altres coses, assignar també unitat de xarxa. 
Fins i tot quan no funcione la detecció de xarxes (el problema de dependències tractat al punt 2.5 podem accedir a les carpetes compartides a través de la xarxa fent ús dels comandaments *Net use*. 
Net use estableix una connexió directa basad en el protocol SMB (Samba) i no usa els altres protocls al·ludits al punt 2.5.

*Win + R:cmd*

```cmd
net use F: \\WinServ1\Dades2024 /persistent:yes
```

Per veure totes les Unitat de xarxa ( "lletres") assignades
```cmd
net use
```

Per eliminar-ne alguna
```cmd
net use f: /delete
```

# 7 Consola del sistema de fitxers *fsmgmt.msc*

La consola *fsmgmt.msc* ens permet

* Tancar fitxers oberts en la xarxa
* Veure els usuaris de xarxa que estan accedint-hi (sesiones)
* Veure els recursos compartits amb el nom que es comparteixen. Si acaba amb $ són ocults.

![*Consola de sistema de fitxers*](png/ADDS/fsmgmt.png)



# 8 Nota final sobre els prototocols en Windows Server

**Protocols**

La detecció de xarxes en Windows es basa en una combinació de protocols (LLMNR, NetBIOS, SSDP) i serveis com. Tenim, per tant, unes "dependències".
L'Explorador de equipos En canvi, el comandament **net use** usa el protocolo **SMB  Samba** per establiis una **conexió directa** con el recurs compartit.
També hem vist que podem usar el **ICMP4** fent un ping. 

**Com a servicis**

Per una banda veiem que podem habilitar-los com a serveis i, una vegada habilitats, iniciar-los o apagar-los ( també inici automàtic).
*Wind +R : services.msc*

**Firewall**
El Firewall no sols pot bloquejar "apliacions" com la *detecció de xarxa* o *compartició de fitxers i impressores* que hem vist. També ens permet establir regles d'entrada o eixida per a cadascun del protocols.

![*Vista del Firewall*](png/ADDS/ExemplesEnFirewall.png)