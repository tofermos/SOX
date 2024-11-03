
# 1 INTRODUCCIÓ

En xarxes amb volumns de dades compartits entre usuaris pot interessar l'establiment de límits d'emmagatzematge. Aquests límits assignats als usuaris que impedeixen un mal ús o abús dels recursos quan algun usuari ocupa una quantitat massa gran d'espai d'emmagatzematge són les *quotes*.
compartit.
En aquesta unitat les estudiarem al Domini basat en Windows Server i més avant en Linux Server.

# 2 QUOTES EN WINDOWS

Les quotes de disc realitzen un seguiment i controlen l'ús de l'espai de disc en els volums NTFS.

* Evitar que s'utilitze més espai de disc i registrar un succés quan un usuari sobrepasse un límit d'espai
en disc especificat, és a dir, la quantitat d'espai de disc que pot utilitzar un usuari.

* Registrar un succés quan un usuari sobrepasse un nivell d'advertiment d'espai de disc especificat, és
a dir, el punt en què un usuari s'acosta al seu límit de quota.

Quan habilites quotes de disc, pots configurar dos valors:

* El límit de la quota de disc

* El nivell d'advertència de la quota de disc.

Per exemple, pots configurar un límit de quota de disc de 100 megabytes (MB) i un nivell d'advertència de
quota de disc de 80 MB. En aquest cas, l'usuari no pot emmagatzemar més de 100 MB d'arxius en el volum (volum).
Si l'usuari emmagatzema més de 100 MB d'arxius en el volum, es pot configurar el sistema de quotes de disc
per que registre un succés del sistema. 
Evidentment l'administració de quotes en un volum requereix drets d'administrador en el Servidor (algun software de configuració tocarem) però també estem condicionant o limitant els drets del usuaris del domini, per tant, haurem de ser un usuari Administrador del domini. ( El grup Admins.del dominio, per exemple).

A partir del moment en el qual s’habiliten quotes de disc per a un volum, es realitzarà automàticament un
seguiment de l'ús quantitatiu del volum que facen tots els usuaris.
Pot ser útil habilitar quotes i no limitar l'ús de l'espai de disc sinó sols realitzar un seguiment de l'ús de l'espai de disc per part de cada usuari. També podeu especificar si ha de registrar o no un succés quan els usuaris superen el seu nivell d'advertència de quota o el seu límit de quota.

Les unitats extraïbles i els volums de xarxa s’han de compartir, aquests últims des del directori arrel del volum. El programa d'instal·lació de
Windows actualitza automàticament els volums que tinguen el format NTFS utilitzat a Windows NT 4.0.

Amb la compressió d'arxius no eludim el control si configurem aquest en local, ja que el seguiment es fa atenent a la seua grandària original, sense comprimir.

Per contra, en fer un seguiment de l'ús del volum en les carpetes compartides, Windows calcula el límit de quota mitjançant la grandària comprimida. 

### No interacció entre usuaris

La utilització de l'espai de disc que faça cada usuari, no afectarà a les quotes de disc d'altres usuaris del mateix
volum. 
Per exemple, si un usuari guarda diversos arxius amb una mida de 100 megabytes (MB) al Volum E,
que té un límit de quota de 500 megabytes, no podrà escriure més dades en el volum si no elimina o mou
abans diversos dels arxius existents. Tanmateix, la resta dels usuaris encara poden guardar en aquest volum.
Les quotes de disc es basen en **el propietari** dels arxius indistintament dels directoris on s'ubiquen el fitxers.

### Queotes en volumns

Les quotes de disc només afecten als volums i són independents de les estructures de les carpetes dels
volums i de la seua distribució en discs físics.

Si un volum té diverses carpetes, les quotes assignades a aquest volum afecten totes les carpetes. Per
exemple, si \\Produccio\QA i \\Producció\Públic són carpetes compartides en el Volum F, els arxius
emmagatzemats pels usuaris d'aquestes carpetes no poden sobrepassar l'espai de disc establert en el límit
de quota del Volum E.
Si un sol disc físic conté diversos volums i aplica quotes a tots els volums, cada quota de volum afecta
exclusivament a aquest volum. Per exemple, si comparteix dos volums distints, com Volum E i Volum G, es fa
un seguiment independent de les quotes per als dos volums, encara que es troben en el mateix disc físic.
Si un volum abasta múltiples discs físics, s'aplica la mateixa quota a tot l'àmbit del volum. Per exemple, si el
Volum F té un límit de quota de 500 megabytes (MB), els usuaris no podran guardar més de 500 MB en aquest
volum, independentment de si resideix en un disc físic o abasta tres.

# OPERATIVA

## 3.1 HABILITAR QUOTES DE DISC
Fer clic amb el botó secundari del ratolí en el volum per al qual vulgueu habilitar quotes de disc i, a
continuació, Propietats. En el quadre de diàleg Propietats, feu clic a la pestanya Cuota. A la pàgina de
propietats de Quota, activeu la casella de verificació Habilita l'administració de quota i seleccioneu una o
diverses de les següents opcions:


* **Denegar espai de disc als usuaris que sobrepassen el límit de quota**. Els usuaris que sobrepassen
els seus límits de quota rebran l'error "espai insuficient en el disc" de Windows i no podran escriure
més dades en el volum si no eliminen o mouen abans un o diversos arxius.
Cada programa tracta aquest error d'una manera específica. Per al programa, serà com si el volum
estigués ple. Si desactiveu aquesta casella de verificació, els usuaris poden sobrepassar el límit de
quota. Quan sobrepassem el valor màxim (per exemple, quan copiem un arxiu) se’ns mostrarà l’avís
d’error:

* **Limitar espai de disc a.** Escriviu la quantitat d'espai de disc que poden utilitzar-se als nous usuaris del
volum i la quantitat d'espai de disc que s'ha d'utilitzar per que s'escra un succés en el registre del
sistema. Els administradors poden veure aquests successos en el Visor de eventos. És possible utilitzar
valors decimals (per exemple, 20/5). Pel que fa als nivells d'espai, i advertiment, seleccioneu les
unitats apropiades en la llista desplegable (per exemple, KB, MB, GB, etc.).
Registrar succés quan un usuari excedisca el seu límit de quota. Si s'han habilitat les quotes, s'escriu
un succés en el registre de sistema de l'equip local cada vegada que un usuari sobrepassa el límit de
quota. Els administradors poden veure aquests successos en el Visor de eventos.
Per defecte, els successos de quota s'escriuen cada hora en el registre del sistema de l'equip local. Si
voleu canviar l'interval d'escriptura dels successos de quota en el registre del sistema de l'equip local,
utilitzeu el comando fsutil behavior. (fsutil behavior set quotanotify 5, per exemple, establirà que
cada 5 segons s’actualitze el visor d’esdeveniments en quant a les notificacions de quota; reinicia
després de canviar aquest paràmetre)

* **Registrar succés quan un usuari excedisca el seu nivell d'advertiment**. Si s'han habilitat les quotes,
s'escriu un succés en el registre de sistema de l'equip local cada vegada que un usuari sobrepassa el
seu nivell d'advertiment. Els administradors poden veure aquests successos en el Visor de eventos.
Per defecte, els successos de quota s'escriuen cada hora en el registre del sistema de l'equip local. Si
voleu canviar l'interval d'escriptura dels successos de quota en el registre del sistema de l'equip local,
utilitzeu el comando fsutil behavior.

* El visualitzador de successos crea un registre històric en ordre cronològic dels usuaris que han
sobrepassat els límits de quota i nivell d'advertència de quota, i en quin moment. En canvi, no
proporciona informació sobre els usuaris que es troben actualment per sobre del nivell d'advertència
de quota. Per a saber aquesta informació, amb el botó dret sobre el disc, a la pestanya Cuotes s’ha
de premer el botó Entrades de quota.

# 4 QUOTES DIFERENCIADES SEGONS USUARIS

Obri l’explorador de Windows i selecciona la unitat de disc per a la qual vulguem habilitar quotes diferents
segons l’usuari. Amb el botó dret del ratolí selecciona propiedades i a la finestra emergent (se suposa que ja
estan habilitades les quotes) prem sobre el botó Valores de Cuota.

* A la finestra que apareix anem al menú Cuota>Nueva entrada de cuota

* A la finestra emergent podem escriure l’usuari(s) als quals volem aplicar els valors de quota:

* Li donem a Aceptar i a la nova finestra que apareixerà establim els valors que ens interesse:

## QUOTES A WINDOWS SERVER

La forma de treballar amb quotes a Windows 10 a l’apartat anterior no sembla que siguen molt útils a un
entorn corporatiu, ja que tenim molt poques opcions. 


maginem aquest escenari: al nostre servidor amb
Windows Server li afegim un RAID 5 per emmagatzemar les carpetes personals del usuaris, les carpetes dels
departaments... Evidentment volem controlar l’ús d’aquest volum lògic amb quotes de disc. Malauradament
només podem crear una quota per usuari, és a dir no podem crear una quota per a una carpeta específica.
Per sort, a Windows Server tenim l’eina "Administrador de recursos del servidor de archivos". Per instal·lar-la
utilitzarem com sempre des del panell "Administrador del servidor " l’opció de "Administrar → Agregar roles
y características". I des d’allí afegirem "Servicios de archivos y almacenamiento → Servicios de iSCSI y archivo
→ Administrador de recursos del servidor de archivos".
Obrim la eina i dintre d’administració de quotes tenim dues opcions:
▪
▪
Quotes: Mostra totes les quotes creades al "Administrador de recursos del servidor de archivos". Si
fem botó dret →crear nova quota, podrem definir una al nostre gust, ja siga partint de zero o
utilitzant les plantilles disponibles (opció recomanada).
Plantilles de quotes: Mostra les plantilles existents i ens permet crear de noves. Podem configurar
la grandària de la quota, i si volem que ens avise per correu electrònic o que genere un registre al
visor d’esdeveniments.

84.1 CREAR PLANTILLES
Per a crear plantilles, s’ha de fer botó dret bé siga en “Plantillas de cuota” o en la finestra on apareixen les
plantilles creades:
▪
▪
▪
▪
▪
▪
Copiar propietats de la plantilla de quota (opcional): Permet crear una plantilla a partir d’una altra.
Nom de plantilla: Es defineix el nom que vullgam que tinga la plantilla.
Descripció (opcional).
Límit d’espai: On definirem el límit de la quota.
Tipo de quota:
o Quota màxima: No permet als usuaris superar el límit
o Quota d’advertència: S’utilitza per a tindre un control de l’espai utilitzat. Permet als usuaris
superar el límit.
Umbral de notificació: Per a definir el umbral de notificació s’ha de prémer sobre Agregar
9o
Es pot configurar que s’envie un correu al administrador i/o al usuari que ha superat el
umbral de quota. Si no tenim un servidor SMTP configurat ens donarà un avís (li donem
a Acceptar).
10o
També es pot configurar que es genere un registre d’esdeveniments quan es supere el
umbral.
4.2 CREAR QUOTES
Una vegada creada la plantilla ja podem aplicar-la a algun directori. Des del mateix Administrador de recursos
del servidor de archivos anem a Administración de cuotas - Cuotas, fem clic amb el botó de dret i seleccionem
Crear cuota... Seleccionem la ruta d’accés a la que anem a aplicar la quota, activem el selector de Aplicar
plantilla autom. y crear cuotas en subcarpetas nuevas y existentes i triem la plantilla que hem creat:
11I observem les diferents entrades que s’han creat al sistema de quotes en aplicar la plantilla:
4.3 COMPROVACIÓ DEL SISTEMA DE QUOTES
Per a comprovar que funciona, el que farem primer que res es modificar la freqüència amb que es generen
els esdeveniments de quota, ja que, com hem vist a Windows 10, per defecte els successos de quota
s'escriuen cada hora en el registre del sistema de l'equip local. Aleshores, per a canviar-lo farem ús del
comando fsutil behavior set quotanotify numero_segons (per a que els canvis siguen efectius s’ha de reiniciar
el servidor).
En Windows, per a crear fitxers d’una grandària determinada podem utilitzar el comando:
fsutil file createnew <nom del arxiu> <grandària en bytes>
Després accedim a una màquina client i anem a muntar en una unitat de xarxa el directori, per a copiar arxius
des de la terminal.
12Obrim una terminal i creem arxius de diferent grandària al directori personal de l’usuari. Després, accedim a
la unitat de xarxa i copiem els fitxers que em creat:
13Després, anem a comprovar en el Servidor com estan les quotes. Observem que la quota de datosAlmacen
està al 110%:
En el Visor de eventos apareixeran dos esdeveniments, un d’advertència de que l’usuari ha superat el umbral,
i un altre d’error perquè no ha pogut enviar el correu electrònic:
14