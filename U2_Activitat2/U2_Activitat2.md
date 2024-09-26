---
title: U2.Instal·lació i ús de Windows Server
subtitle: Activitat Unitat 2
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
> Abans de començar, estudieu la Unitat 2 i llegiu TOTA la pràctica. Hi pot haver un apartat que no pugues contestar fins el final o una dada que te la proporcionen després.


# Descripció

No aneu a cerar encara un domini però tots sí tots els primers passos per a fer-ho. 

## Pas 1:

L'objectiu de la activitat és connectar un Windows 1x amb un Windows Server en un Workgroup de forma similar a la pràctica anterior però caldrà, a més :

* Indiqueu la IP de DNS en la configuració del adaptadors.
* Assegureu-vos la compartició i detecció de xarxes que suposa més faena (repassa el tema) però 
    Sense deixar un forat en la seguretat.
* Configureu altres qüestions del Servidor tal com heu vist a la Unitat 2 com el Windows Update i indiqueu perquè.


## Pas 2:

Una vegada el WG funcionant, creeu una carpeta compartida en les dos màquines i assigneu una unitat de xarxa.

* En el client, l'assignareu des del GUI (a la carpeta del servidor).

* En el servidor, l'assignareu des del CLI (a la carpeta del client)

Crea un fitxr txt (amb el Bloc de Notas, per exemple) en la carpeta compartida del servidor i obri'l des del client. Força el seu tantcament des del servidor.
