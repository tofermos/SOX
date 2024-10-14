---
title: U3.Administració i configuració del Windows Server. DHCP
subtitle: Activitat Unitat 3. Prova de DHCP
author: "@tofermos 2024"
lang: "ca-ES"
papersize: A4
linestretch: 1.5
output:
  html_document:
    toc: true
    toc_float: true
    toc_depth: 3
    df_print: paged
    number_sections: false
  pdf_document:
    toc: true
    keep_tex: true
    latex_engine: xelatex
---

\newpage
\renewcommand\tablename{Tabla}

> NOTA PRÈVIA:
>
> Abans de començar, estudieu la Unitat 3 i llegiu TOTA la pràctica. Podeu continuar a partir de la Activitat anterior.

# 1 Descripció

Activitat per provar l'usuari del domini i el servei DHCP.

# 2 Pasos 

En aquesta activitat farem:

* Provar que l'usuari del domini pot iniciar sessió des de 2 PC clients. 

* Veure les sessions i els fitxers des del servidor (*fsmgmt.msc*)

* Configurar DHCP ( Servidor i clients) i provar (reiniciant adaptador o PC) que funciona.

* Fer una reserva d'una IPv4 a l'aderça MAC d'un PC.


## Opcional

Es tracta de provocar dos errors per entendre millor el funcionament del DHCP.

* Fent ús de la Reserva i una assignació automàtica, podeu reproduir un error de duplicitat en les IP.

* Canvieu el tipus d'inici del Servici DHCP del client a *manual*. Després el *detenir-lo* i reinicieu el PC.

Funciona? Prova canviar el temps d'assignació de IP del servidor i reinicia després que caduque.

# 3 Objectius

Bàsicament anem a comprovar:

* Avantatge de la centralització de comptes (usuari del domini) vs comptes locals en xarxa.

* El funcionament més bàsic del DHCP com a servei de Windows Server en clients Windows 1x.


