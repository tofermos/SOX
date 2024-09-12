# PRÀCTICA FINAL AMB WINDOWS SERVER 2019
## DESCRIPCIÓ
L'activitat consisteix en la configuració de part d'una LAN model Client / Servidor amb un Windows Server 2019 per a una empresa de desenvolupament d'aplicacions multiplataforma.
Organitzarem els usuaris i els ordinadors clients atenent a la seua funció i, estos, al seu temps, s'agruparan en Unitats Organitzatives segons el tipus tipus de projectes a desenvolupar. És a  dir, les UO es crearan en base a un criteri organitzatiu.


### 1.  L'organització final dels objectes quedarà amb l'estrutura de **Unitats Organitzatives**  i **Grups** següent:

| GRUP|USUARI|
|--|--|
|grDisseny_01|tomas, ariadna, PC01|
|grProgramacio_01|aitana, rosa, PC01|
|grDepuracio_01|isabel|
|

*( els grups també contenen algun PC client )*

|UO|entitats|
|--|--|
|uoProjectesERP|grDisseny_01, grProgramacio_01, grDepuracio_01|
|uoProjectesFARMACIA|||
|


### 2.  Tots els usuaris tindran **PERFILS MÒBILS** en una carpeta *PERFILS*

*(En les còpies de seguretat s'inclouran els perfils dels usuaris)*


### 3.  Hi haurà 3 **CARPETES COMPARTIDES** amb la següent configuració de permisos ( **NTFS** ) tal que:

 ||grDisseny|grProgramacio|grDepuracio|Administradors|
 |--|--|--|---|--|
 |ANALISI|Modificar, Escriure, Llegir, Executar, Llistar |Llegir, Executar, Llistar| Llegir, Executar, Llistar|Control Total|
|CODIFICACIO|Llegir, Executar, Llistar|Modificar, Escriure, Llegir, LListar i Executar | Modificar, Escriure, Llegir, LListar i Executar. NO *Crear arxius / Carpetes Ni Escriure Dades/ annexar*|Control Total|
 |DEPURACIO|Llegir, Executar, Llistar| Llegir, Executar, Llistar| Modificar,Escriure, Llegir, LListar i Executar|Control Total|
|
### 4.  La contrassenyes dels usuaris hauran de complir les restriccions següents:
*   Ser complexes
*   Longitud mínima de 8 caràcters.
*   Durada màxima de 45 dies.

# GUIA PER RESOLDRE L'ACTIVITAT

>Heu de fer una captura de les comprovacions demanades en text ombrejat.
## PRIMERA PART:    Instal·lacions, Accés Remot i Domini.

### Instal·lació 

Instal·lació de les màquines virtuals.
*  Windows 10 Pro.
*  Windows Server 2019.

Asssegurar que hi puga haver connectivitat:
1.  Configurar les MV com a Xarxa Interna
2.  IPs privades en la mateixa xarxa ( Per exemple: 
    IP Windows 10 Pro: 192.168.0.2 /24
    IP Windows Server; 192.168.0.1 /24
3.  Llevar les restriccions del FireWire de Windows.

>Comprovar que tenim connectivitat fent **ping** i **ipconfig**.

### Accés remot

1.  Des d'un client ( Windows 10 Pro ) creem una Connexió Remota amb el Servidor. Necessitareu:
La IP del servidor
2.  Autenticar-vos com usuari local del servidor ( usareu l'usuari Administrador del Servidor )

>Inicieu una sessió remota des del client i entreu a l'explorador del Servidor.

### Crear el Domini.
De moment el que tenim és una LAN del tipus Work Group ( grup de treball ). Usuaris locals de cada màquina i cap administració centralitzada. Per passar de WORKGROUP -> DOMAIN cal que:

1.  Instal·leu el Active Directory
2.  Promocioneu el servidor.

Per al nom de domini podeu usar el vostre cognom afegint el ".LOCAL": COGNOM.LOCAL 

> Visualitzeu i comproveu el Domini en **Usuaris i Equip d'Active Directoy**, podeu entrar executant **dsa.msc**


## SEGONA PART: L'Active Directory.

### Afegir el PC client al Domini.

Canvieu el nom del PC Client i el Domini al qual pertany.
Vos demanarà un compte amb drets d'Administrador en el Domini.

Podeu entrar executant el **dsa.msc**

> Comproveu que en l'Active Directory apareixen els PCs clients com entitats del domini.

## Crear les entitat que falten al domini.

Useu este ordre: 

1.  Usuaris del domini.
2.  Crear les UO
3.  Crear els grups dins de les UO
4.  Incloure els usuaris i PC en els grups corresponents.

## TERCERA PART: Directives de Windows.

### Crear la GPO per a les contrassenyes dels usuaris.

Caldrà crear una nova Directiva de Grup. Podem entrar executant **gpmc.msc**
Creem una directiva de grup que vincularem a la UO que ens interesse o al Domini.

Recordem que hem d'executar **gpupdate /force** per a que els canvis tinguen efecte.
> Des d'*Usuaris i equips de l'AD* , executeu *Restablir Contrassenya* d'algun usuari i proveu la nova directiva.

## QUARTA PART. Permisos i compartició de carpetes.

### Crear, compartir carpetes i assignar de permisos a les carpetes.

Useu la consola **fsmgmt.msc** de CARPETES COMPARTIDES per crear, compartir i assignar permisos NTFS.

Configureu els permisos NTFS especials inclosos. 
Assignem permisos **NTFS** per a cada grup seguint les especificacions.

>Comproveu canviant de sessió des d'un PC client

Des d'esta consola podem gestionar molt millor els recursos compartits ( vore els ocults, eliminar algun que ha desaparegut o canviat, tancar algun fitxer o sessió que haja quedat oberta... ).



## CINQUENA PART: Perfils mòbils.

Si només tenim un PC Client. Haurem d'iinstal·lar un segon per poder fer les proves de perfils mòbils.
Recordeu, importeu o cloneu:

*   IP pcClient02 192.168.0.3 /24
*   DNS: 192.168.0.1/24
*   Reviseu el Firewall.

1.  Crear un carpeta Perfils.
2.  Compartir-la i donar permisos d'escriptura als usuaris del domini que vullguem.

> Comprova usant els dos client amb un usuari.









