---
output:
  html_document: default
  pdf_document: default
---

# SCRIPT EN POWERSHELL.

### 1. Crea una Unitat Organitzativa.

Nom: aula206
Guarda el nom de la UO en una variable $UNITAT

### 2. Crea un grup dins de la UO.

Nom: SMX2B
Guarda el nom del gruop en una variable $GRUP

### 3. Crear un usuari (amb password prèviament) i afig-lo al grup.

A partir d'un script que parcialment et deixe ací, completa'l per a que puga llegir d'un fitxer `.csv` i crear usuaris de forma massiva.

L'estructura del `.csv` serà:

```
LOGIN, PASSWORD
Hugo, Gandia2425
Arantxa, Gandia2425
Gabriel, Gandia2425

```

## Ajuda: Crear una contrasenya amb `ConvertTo-SecureString`

```powershell
# Crear una contrasenya segura
$pass = ConvertTo-SecureString -String "ContrasenyaSegura123!" -AsPlainText -Force
```

## Completar script

L'script que heu de completar és el següent.
Heu d'usar les vaiaables $UNITAT i $GRUP, també 


```powershell
clear-host
# ANEM A IMPORTAR UN FITXER CSV ON TINDREM NOM i PASSWORD D'USUARIS A CREAR

$fitxerUsuaris = Import-Csv -Path "ruta_del_fitxer.csv"
foreach ($usuari in $fitxerUsuaris) {
  # Amb la variable $usuari accedirem a cadascun dels dos camps
    # Crear una contrasenya segura
    $password = ConvertTo-SecureString -String $usuari.PASSWORD -AsPlainText -Force
    
    # Crear l'usuari local
    New-LocalUser -Name $usuari.LOGIN ...
    
    # Afegir l'usuari al grup
    Add-LocalGroupMember -Group ... -Member ....
}
```

---

Aquest script llegeix les dades d'un fitxer `.csv` i crea usuaris locals al sistema amb les contrasenyes indicades. Finalment, afegeix els usuaris al grup especificat (`grup1`).

