---
title: U3. WINDOWS SERVER. ADMINISTRACIÓ I CONFIGURACIÓ
subtitle: Perfils Mòbils, Carpetes particulars i readreçament de carpetes
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

# Perfils mòbils.

La configuració de Perfils Mòbils serveix per guardar al Servidor la configuració del perfil de l'usuari. 
Podem aplicar-ho a usuaris o grups concrets o a una Unitat Organitzativa o a tots els usuaris del Domini. De moment en esta unitat anem a fer-ho de forma individual a un usuari.

Avantatges:

* Possibilitat d'iniciar la sessió en qualsevol PC client i disposar del teu perfil (Configuració d'Escriptori, Unitats de xarxa...)

* Possibilitat de que l'Operador de còpies de seguretat programe un backup del nostre perfil.
  
Desavantages:

* La càrrega del perfil des del servidor fins el PC client genera un tràfic a la xarxa local que pot notar-se en iniciar tots el usuaris al mateix temps.

* Els usuaris poden despreocupar-se del tamany de les seues dades i acabar, entre tots, ocupant massa espai de disc dur al servidor.




## Crear i compartir carpeta

* Creem una carpeta en el servidor que compartim...

* ...donant permisos **control total** als usuaris als que volem configurar un perfil mòbil per a que en el primer inici de sessió puguen crear una subcarpeta i prendre possessió i guardar fitxers.

* La resta d'usuaris del domini no deuen accedir a una informació de perfil.

![*Figura1: Compartir una carpeta en el Servidor*](png/CarpetaPerfilsMobils.png)

### Assignem la ruta del perfil

En el *dsa.msc* assignem el la ruta de la carpeta a un usuari (Figura 2)
Si seleccione, més d'un usuari (seleccionat amb Control) podem fer-ho de colp amb la variable **%username%** (Figura 3)

* Per a un usuari podem usar el *nom d'usuari* ( a la imatge usar la variable %username% )

![*Figura2: Assignem la ruta del Perfi Mòbil a un usuari*](png/RutaPerfils.png)


* Per a assignar a més d'un usuari al mateix temps ens valem de %username% 

![*Figura3: Assignem la ruta del Perfi Mòbil a más d'un usuari*](png/RutaPerfils2.png)


# Carpetes particulars

Son carpetes del Servidor que poden ser emprades per desar els treballs de cada usuari a efecte de centralitzar els backups. Ací es tracta més de dades del treball del dia a dia que de la confiuguració.

Ho fem de forma anàloga a l'anterior amb els usuaris que vullguem.

![*Figura4: Compartir una carpeta en el Servidor*](png/CarpetaCarpetasParticulares.png)

![*Figura5: Assignem la ruta de la Carpeta Particular a un usuari*](png/RutaCarpetaParticular.png)

També es pot aplicar a més d'un de colp amb *%username%*



>Nota
>
>Moltes d'estes funcionalitats que estem aplicant de forma individualitzada, més avant vorem que es poden (i deuen) fer de forma més automatizada per atots els usuairs o PC de les UO o Domini.
>Ací és on entren en joc els scripts amb cmdLets de PowerShell i les Directives de grup.


# Readreçament de carpetes

També és interessant que les carpetes habituals de treball com són "Documentos", "Baixades" etc. les puguem tindre guardades al servidor i que també puga l'administrador del domini o l'operador de backups fer una còpia d'elles.


## Crear la Directiva de Grup

Ara treballarem en la consola de directives de grup: **gpmc.msc** ( **Administración de Directivas de Grupo**)

Podem seleccionar la **UO (o el Domini sencer)** on volem que s'aplique el nou adreçament. O crear la directiva i després vincular-la.

![*Figura6:Creem un nova directiva*](png/CrearGPORedireccionar.png)

![*Figura7:L'editem*](png/EditarGPORedireccionar.png)

![*Figura8:Busquem en la plantilla el que volem modificar](png/PropiedadesGPORedireccionar.png)

![*Figura9:Sols per a un grup: Básico*](png/Basico.png)

### Una carpeta per usuari

Independentment de la carpeta (Documents, Baixades...), podem optar per separar la informació de cada usuari.

Ací per crear una carpeta per cada usuari NO hem d'indicar el nom ni %username%

![](png/CrearCarpeta.png)

![](png/CrearCarpetaRuta.png)


### Compartir tot en un sola carpeta

Independentment de la carpeta (Documents, Baixades...), podem optar per compartir en una sola carpeta.

![](png/CrearCarpetaRuta2.png)


# gpupdate

Recordeu:
```cmd
gpupdate /force
```

Si teniu iniciada la sessió d'un usuari tindreu este error:

![](png/gpupdateError.png)

Tanquem la sessió i repetim el *gpupdate /force*


# Exemples d'ús

Optem per carpetes particulars amb accès a través d'una unitat de xarxa (P:) on cada usuari té una carpeta. Indiquem que no usen Documentos per a informació de l'empresa. També podem ocultar la carpeta Documentos en el client (com vàrem vore el curs passat en SOM).

Alternativament podem readrecem Documents a una carpeta compartida i no usar les Carpetes Particulars.

O bé manteir les dos opcions. Carpeta Particular per a un tipus d'informació i Documents per a altra.

Independentment, el Perfil pot ser o no mòbil.

# Consideracions finals

## Relació amb les Unitats Organitzatives (UO)

El que hem vist es exemple d'utilitat de les Unitats Organitzatives. Recordem que les UO són agrupacions d'objectes (usuaris, grups, altres UO, carpetes...) a efectes d'administració i transparents a l'usuari final.

L'usuari guarda en "Documents" o en "P:" sense preocupar-se d'on ni saber com treballa el company. Són els administradors qui han assegurat que, encara que canvie de PC o es connecte amb un portatil al Domini, sempre accedirà a les mateixes carpetes de dades i així controlaran millor la còpia de seguretat o neteja de fitxers temporals.

## La solució depén del cas

No existeix una forma "correcta"  de configurar. Els administradors de la xarxa han de tindre en compte les necessitats de l'organització, dels usuaris i els seus costums per decidir:

1.  **Quina informació** convé desar al Servidor i fer **backup periòdicament** i quina pot quedar-se al PC Client per no tindre valor per a l'organització.

2. A partir d'ací, també hauran de marcar unes **normes d'ús** per als usuaris. Indicant-li on ha de guardar allò que és important per l'organització i on pot (si pot) deixar les "seues coses" o informació temporal sense valor.  Es pot configurar que es guarde al Servidor, fins i tot, les descàrregues o altres fitxers temporals a efectes com hem vist.



