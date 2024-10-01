---
title: U3.Adminsitració i configuració del Windows Server
subtitle: Activitat Unitat 3. Creació de domini
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

> NOTA PRÈVIA:
>
> Abans de començar, estudieu la Unitat 3 i llegiu TOTA la pràctica. Hi pot haver un apartat que no pugues contestar fins el final o una dada que te la proporcionen després.


# Descripció

En aquesta activitat anem a crear el domini. Abandonarem el model de WorkGroup i passarem a una arquitectura clarament ja de Client/Servidor.

> NOTA
>
> On diu *nomteu* has de posar el teu cognom primer o nom de pila

## Pasos per crear el Domini

Seguiu la documentació del repositori (U3 Administració i configuració II):

* Instal·lar el AD-DS
* Promocionar el Windows Server a DC
* Crear un nou bosc amb el nom de domini rel:
   *nomteu.local*
*Ens assegurem que el servidor serà el Servidor DNS.

## Pasos per crear algun objetes del AD

* Crea una UO amb el nom "usuari_DelegacioNord"
* Crea un grup d'usuaris de la delegació creada amb el nom "gr_programadors"
* Crea un usuari pertanyent al grup amb el nom: "nomteu_programador"
* Assigna-li una contrassenya

# Afig el Windows 1x al domini

Afig el PC al domini.
Inicia sessió amb l'usuari del domini creat

1 Accedeix a: *F:\\DADESSERVIDOR\\f1.txt* compartida del servidor i obri'l des del client. 

2 Deixa'l obert en Windows 1x. En l'apartat "Sesiones" de *fsmgmnt.exe*. Ara, veus alguna sessió del Windows 1x? 

3 Força el tacament del fitxer obert en "Ficheros abiertos"

4 Comprova que s'ha tancat.



