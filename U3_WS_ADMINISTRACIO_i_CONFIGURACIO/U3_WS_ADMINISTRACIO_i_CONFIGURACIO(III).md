---
title: U3. WINDOWS SERVER. ADMINISTRACIÓ I CONFIGURACIÓ (III)
subtitle: AFEGIR PC CLIENT AL DOMINI
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

## 1- Revisem connexió del PC-Servidor

>Nota:
>Assegurem que tenim el PC client i el Servidor poden compartir dades com a Workgroup.

>
>*  Els cables estan ben connectats amb el llum de les targes OK.(Xarxa interna del Virtualbox)
>
>*  IP del mateix rang que la del servidor (provem ping)
>
>*  La IP del DNS ha de ser la del servidor DNS
>
>*  Compartir i detectar xarxes activat
>
>     * Revisem problema de dependències de serveix (protocols)
>
>     * Firewall permetent la compartició de fitxers i detecció de xarxa
>
>     * Aplicacions
>
>     * Regles
>     
              
Podem partir del que tenim fet de la unitat anterior o fer un non Workgroup amb carpetes compartides i assignades a unitats de xarxa.

## 2- Afegim el client al Domini

*Panel de Control, Sistema, Cambiar configuración*
...
Més cerques
o

*Este Equipo, Propiedades...*

![](png/18.png)

Vos demanarà un **compte amb drets d'Administrador en el Domini**.
Podem usar el que hem creat en la instal·lació del Server.

![](png/demanacompteadministradordeldomini.png)


:computer: En el SERVIDOR, podeu visulitzar el PC afegit al Domini en **Usuaris i Equips d'Active Directoy**

Podeu entrar executant el **dsa.msc** ( Winn + R ...)

![](png/20.png)

## 3.  Iniciem sessió al Domini des del Client

Per iniciar sessión en el domini des del Windows 1X, encara que podríem usar algun usuari de domini predefinit com el mateix compte d'Administrador, el que farem prèviament és crear un usuari nou. De fet el normal és disposar de comptes per als usuaris reals de l'organizació.

**3.1 Creem l'usuari**

Des de l'Administrador del Servidor veiem les dos funcionalitats des d'on podem accedir a la gestió de comptes.

![](png/centroadministracionactivedirectory.png)


Centro de Administración del Active Directory

![](png/administradordelservidorADDS.png)

Usuarios y equipos del AD

![](png/usuariosyequiposactivedirectory.png)

**Contrasenya amb requisits de complexitat**

Si intentem indicar una contrasenay senzill, curta, fàcil... vorem que no ens ho permet el SO. Més avant tractarem estos requisits i altres característiques de la contrassenya i dels comptes. 

![](png/creausuaridomini.png)

![](png/creausuaridominicontrasenya.png)

**El grup "Usuarios del dominio"**

Observem que tots els usuaris del domini perntanyen a un grup preexitent "Usuarios del Dominio".

![](png/grupousuariosdeldominio.png)

**Iniciem sessió**

Com la màquina client ja pertany al domini, en iniciar sessió del Windows 1x només cal selecionar un usuari del domini nou usuari.

>Nota
>
>Més avant vorem amb un poc més de detall la configuració de les entitats del AD i les seues relacions. També tenim documentació per a la seua gestió mitjançant cmdLets i scripts fets amb Powershell
>

**Característiques dels usuaris de domini al client**

* El nou usuari (usuari1) tindrà un perfil al PC Client però

* NO és Administrador del PC Client. Qualsevol canvi de configuració que intente fer, el Windows 1x li demanarà les credencials d'Administrador local ("tomas", a l'exemple)

* Quan vullguen inicar sessió amb l'Administrador del Windows 1x, devem canviar de "domini", això ho farem avantposant al "login" el **nom de la màquina local** (a l'exemple: "PCCLient\\tomas" )

* Amb este usuari, si es podria fer Administrador Local ( del Windows 1x ) a l'usuari del domini ( "usuari1")

* Quan iniciem sessió en el domini, caldrà fixar-se i, si cal, avantposar també el **nom netbios del domini** ("FERRANDIS\\usuari1")
