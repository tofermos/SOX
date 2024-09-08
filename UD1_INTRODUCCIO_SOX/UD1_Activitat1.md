title: UD1. Introducció als sistemaes operatius en xarxa
subtitle: Activitats
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

# 1 Activitat 1. Sobre la xarxa en VirtualBox

## 1.1 Modes de la xarxa

Després d'instal·lar un Windows 1x comprova les diferents configuracions de la xarxa i ompli la Taula "Resultat".

Per a tal fi has de:

* Provar totes les opcions i fes un ifconfig des del CMD per comprovar la IP. 
* Mirar també quina IP té el teu PC anfritrió (Ubuntu a l'aula o Windows a casa).
* Fer un ping a la IP de l'amfitrió des de la MV.

*No calen captures de pantalla. Però has de saber bé què has fet i per a què.*

|Té accés a internet|Pot comunicar-se amb altres MV|Es pot comunicar amb la màquina real|Hem de configurar alguna cosa a Linux/Windows|
|NAT|XARXA INTERNA|PONT|
|:--|:--|:--|:--|
|IP MV||||
|IP MV||||
|IP Amfitrió INTERNA||||
|Es comuniquen?(ping)||||

## 1.2 Xarxa local

Anem a unir dos MV Windows 1x per fer un WorkGroup. Fes els canvis que cregues.
*Captura*

# 2 Revisa el software de sistema

Revisa que els dos Windows no tenen cap restricció del software del sistema que els impedisca compartir algun recurs.

*Captures*

# 3 Botiga amb 2 PC

## 3.1 Crea la següent configuració

|Nom del PC|IP|Recurs compartit|Usuari local|
|:--|:--|:--|:--|
|PC-Mostrador|192.168.0.1/24|C:\\DADES\\VendesDia|Joan|
|PC-Oficina|192.168.0.2/24| |SraRoser|

## 3.2 Compartició controlada.

Assegura't que en la carpeta compartida del mostrador, a banda de Joan, només por accedir la gerent (SraRoser) pero sense poder modificar, sol llegir.

## 3.3 Respon de forma raonada als següents dubtes.

a.  Podríem compartir un altra carpeta en C:\users\
