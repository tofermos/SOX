---
title: |
  | U8-ADMINISTRACIÓ D'UBUNTU (at i cron)
subtitle: |
  |  Programar tasques. **at i cron**
Author: "Tomàs Ferrandis Moscardó"
output:
  pdf_document: 
    toc: false
    keep_tex: true
  html_document:
    toc: true
    toc_float: true
    toc_depth: 2
    df_print: paged
    number_sections: false
  word_document:
    toc: true
    toc_depth: '3'
lang: "es-ES"
fontsize: 12 pt
mainfont: Times New Roman
papersize: A4
linestretch: 1.5
---

# Gestió de Quotes de Disc i Còpies de Seguretat a Ubuntu

## 1. Quotes de disc en Linux (Ubuntu) 

Les **quotes de disc** permeten limitar l'espai d'emmagatzematge que un usuari o grup pot utilitzar en un sistema de fitxers. Això és útil per evitar que un sol usuari acapari tot l'espai disponible.  

### 1.1. Instal·lació de les quotes de disc

Ubuntu ve amb suport per a quotes, però cal activar-lo i configurar-lo.  

1. **Instal·lar el paquet de quotes (si no està instal·lat)**  
   ```bash
   sudo apt update && sudo apt install quota quotatool
   ```  

2. **Activar les quotes en un sistema de fitxers**  
   - Edita `/etc/fstab` i afegeix `usrquota` i/o `grpquota` a la partició on vols aplicar quotes.  
   - Exemple per a `/home`:  
     ```
     /dev/sdX /home ext4 defaults,usrquota,grpquota 0 2
     ```  

3. **Reiniciar per aplicar els canvis**  
   ```bash
   sudo reboot
   ```  

4. **Crear els fitxers de quota**  
   ```bash
   sudo quotacheck -cum /home
   sudo quotaon /home
   ```  

5. **Establir quotes per a un usuari (exemple: usuari "jordi")**  
   ```bash
   sudo edquota -u jordi
   ```  
   Això obrirà un editor on pots definir límits "soft" (avís) i "hard" (límits estrictes). Exemple:  
   ```
   Disk quotas for user jordi:
     Filesystem blocks soft hard inodes soft hard
     /dev/sdX  50000 40000 60000 0 0
   ```  

6. **Comprovar les quotes aplicades**  
   ```bash
   quota -u jordi
   ```  

7. **Configurar un avís si un usuari excedeix el límit**  
   ```bash
   sudo warnquota
   ```  

---

## 2. Còpies de Seguretat a Ubuntu 

Fer còpies de seguretat és essencial per evitar pèrdues de dades. Ubuntu ofereix diverses eines, com **rsync, tar, Timeshift, Déjà Dup, Bacula**, entre d'altres.  

### 2.1. Còpies de seguretat amb `rsync` 

`rsync` permet sincronitzar directoris i fitxers de manera eficient.  

- **Còpia local**  
  ```bash
  rsync -av --delete /home/jordi /mnt/backup/
  ```  

- **Còpia remota via SSH**  
  ```bash
  rsync -avz -e ssh /home/jordi usuario@server:/mnt/backup/
  ```  

- **Automatització amb `cron`**  
  1. Edita el crontab:  
     ```bash
     crontab -e
     ```  
  2. Afegeix aquesta línia per fer una còpia cada nit a les 2:00 AM:  
     ```
     0 2 * * * rsync -av --delete /home/jordi /mnt/backup/
     ```  

---

### 2.2. Còpies amb `tar`

Si vols fer còpies comprimides, `tar` és una bona opció:  

```bash
tar -czvf backup_home.tar.gz /home/jordi
```  

Per restaurar:  
```bash
tar -xzvf backup_home.tar.gz -C /
```  

---

### 2.3. Còpies automàtiques amb Déjà Dup  

Si prefereixes una interfície gràfica, `Déjà Dup` (Backup) és una eina fàcil d'usar.  

1. Instal·lar Déjà Dup:  
   ```bash
   sudo apt install deja-dup
   ```  
2. Obrir l'aplicació **Còpia de Seguretat** i configurar la destinació (disc extern, NAS, etc.).  

