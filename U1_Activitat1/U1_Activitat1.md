---
title: U1.Introducció als SOX
subtitle: Activitat Unitat 1
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
> Abans de començar, estudia el tema i llig TOTA la pràctica. Hi pot haver un apartat que no pugues contestar fins  el final o una dada que te la proporcionen després.


# 1 Sobre la xarxa en VirtualBox

## 1.1 Modes de la xarxa

Després d'instal·lar un Windows 1x en le sdos màquines, comprova les diferents configuracions de la xarxa del VisualBox i ompli la Taula "Resultat".

Per a tal fi has de:

* Provar totes les opcions i fes un ifconfig des del CMD per comprovar la IP. 
* Mirar també quina IP té el teu PC anfritrió (Ubuntu a l'aula o Windows a casa).
* Fer un ping a la IP de l'amfitrió des de la MV.

*Fes captures*

||NAT|XARXA INTERNA|PONT|
|:--|:--|:--|:--|
|Té accés a internet||||
|Pot comunicar-se amb altres MV||||
|Es pot comunicar amb la màquina real|||||
|Hem de configurar alguna cosa a Linux/Windows|||||
|IP MV1||||
|IP MV2||||
|IP Amfitrió INTERNA||||
|Es comuniquen les MV(ping)||||
|Es comuniquen amb l'amfitrió||||

## 1.2 Xarxa local

Anem a unir dos MV Windows 1x per fer un WorkGroup. Fes els canvis que cregues.

*Fes captures*

# 2 Sobre la compartició en Windows. 

Revisa que els dos Windows no tenen cap restricció del software del sistema que els impedisca compartir algun recurs.

en acabar, comprova que les dos màquines tenen connexió.

*Fes catptures*

# 3 Botiga amb 2 PC

## 3.1 Crea la següent configuració

|Nom del PC|IP|Recurs compartit|Usuari local|
|:--|:--|:--|:--|
|PC-Mostrador|192.168.1.X /24|C:\\DADES\\VendesDia|elteunom|
|PC-Oficina|192.168.1.(X+1)Y  /24| |elteucognom|

**Indicacions**

* Usaràs el teu nom de pila per a un usuari "elteunom" i "elteucognom" per a l'altre.

* La X de la IP serà la del teu lloc de classe.

## 3.2 Compartició controlada.

Assegura't que en la carpeta compartida del mostrador, a banda de Joan, només por accedir la gerent (SraRoser) pero sense poder modificar, sol llegir.

## 3.3 Respon de forma raonada als següents dubtes.

a.  Podríem compartir un altra carpeta en C:\\users\\elteunom\\"Mis Documentos"
b.  Consideres que és un model de xarxa el Workgroup acceptable
c.  Si hagueres de muntar-lo en la FCT, ¿què necessitaries a banda dels dos PC i una ISO del Windows 11?

