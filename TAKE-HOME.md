---
title: TAKE-HOME. DOMINI WINDOWS SERVER
subtitle: Domini, Unitats Organitzatives, DHCP
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
    number_sections: false
---

\newpage
\renewcommand\tablename{Tabla}



Cada alumne teniu un codi assignat que direm N. 

# 1 Servidor i Creació del Domini

Instal·leu un Windows Server amb un segon disc D: en VirtualBox

Creeu el domini *TAKEHOME-N.local*

Creu una Unitat Organitzativa amb els usuaris, grups i carpetes compartides segon el model.

IP del Servidor 192.168.N.1/24

Nom Servidor: Server*N*

# 2 Unitats Organitzatives

Model A: Cada Unitat Organitzativa agrupa dos departaments que son 2 grups d'usuaris

Model B: Les Unitats Organitzatives responen a regions geogràfiques amb els seus grups d'usaris

Els usuaris pertanyen als grups que heu creat.

(Elimineu, si l'heu creat, l'usuari vboxuser)

**Delegar el control de la UO sobre ususari no administrador**

Un dels usuaris (no administrador) podrà ressetejar les contrasenyes de tots els companys de la UO.

# 3 Carpetes

Les carpetes compartides seran:

D:\DADES2024 -> En el Client F:
D:\DADES2025 -> En el Client G:

Els usuaris dels grups de la UO tindran permís per llegir i escriure en les dos carpetes (la resta d'usuaris, no podran ni llegir).

# 4 DHCP

Instal·la i configura el DHCP al servidor:

1 Rang IP: 192.168.N.1- 192.168.N.100/24

2 Exclusió IP: 192.168.N.1 - 192.167.N.10/24

Les IP excloses serviran per a assignar-se de forma fixa als servidors, router... 

# 5 Client Windows 1x

Instal·la el Windows 1x, afig-lo al Domini i configura'l amb IP dinàmica.

Captura les unitats F: i G: com s'ha indicat al punt 3 de la següent manera:

  1. F: amb net use

  2. G: pel GUI

Averigua la MAC amb
```cmd

ipconfig /all
```

Assigna una IP a la MAC

Reinicia la NIC o el Windows 1x i comprova.

# 6 Models

Els N parells fareu el model A
Els N imparells fareu el model B.

# Rúbrica

La tasca sha de desenvolupar a casa i és un repàs de tot el que hem vist fins a 16/10/2024.
Copiar la tasca d'un company implica el suspens immediat sense recuperació.

1 Configuració de les aplicacions "compartir recursos i detecció de xarxes" correctament.

2 Configuració correcta de les IP de les dos màquines.

3 Configuració correcta del DHCP.

4 Compartició correcta de les carpetes.

5 Captura de les unitats de xarxa.

6 Configuració correcta de les UO.

7 Ubicació del Objectes.

8 Delegació de control.

