---
output:
  pdf_document: default
  html_document: default
---
# Integració de Sistemes Operatius Windows i Linux

## 1. Introducció 
En entorns informàtics moderns, és habitual trobar sistemes Windows i Linux convivint en xarxes corporatives, servidors i fins i tot ordinadors personals. La integració eficient d’aquests sistemes permet aprofitar el millor de cadascun: **Windows** és àmpliament utilitzat en entorns corporatius per la seva compatibilitat amb programari empresarial, mentre que **Linux** destaca per la seva seguretat, flexibilitat i rendiment en servidors.  

La integració de Windows i Linux és possible a múltiples nivells: compartició de fitxers, autenticació, execució d’aplicacions i gestió remota. Amb eines com **Samba, Active Directory, WSL i Ansible**, es pot aconseguir una convivència eficient entre ambdós sistemes, facilitant l’administració i l’automatització en entorns mixtos.

L’objectiu d’aquesta integració és permetre que ambdós sistemes es comuniquin, comparteixin recursos i gestionin usuaris i permisos de manera centralitzada.  


---

## 2. Mètodes d'Integració

L’integració entre Windows i Linux pot abordar-se des de diverses perspectives:  

1. **Compartició de fitxers i recursos**  
2. **Autenticació i gestió d’usuaris**  
3. **Execució d’aplicacions de Windows a Linux i viceversa**  
4. **Gestió remota i automatització**  

---

## 3. Compartició de Fitxers i Recursos

El tema de SAMBA i NFS ja l'hem vist, per tant este tema és un repàs teòric curt i poc més.

### 3.1. Samba: Compartició de Fitxers i Impressores

**Samba** és un protocol que permet a Linux compartir fitxers i impressores amb sistemes Windows.  

#### Instal·lació i configuració bàsica de Samba en Linux
1. **Instal·lar Samba** en un sistema Linux (Ubuntu/Debian):  
   ```bash
   sudo apt update && sudo apt install samba
   ```  
2. **Editar la configuració** a `/etc/samba/smb.conf` per afegir un recurs compartit:  
   ```
   [public]
   path = /srv/samba/public
   writable = yes
   guest ok = yes
   ```  
3. **Reiniciar Samba** perquè els canvis tinguin efecte:  
   ```bash
   sudo systemctl restart smbd
   ```  
4. **Accedir al recurs des de Windows**  
   - Obre `\\IP_DEL_SERVIDOR\public` des de l’Explorador de Windows.  

---

### 3.2. Muntar Recursos Windows a Linux amb CIFS

Si volem accedir a una carpeta compartida de Windows des de Linux, podem muntar-la amb `cifs`:  

```bash
sudo mount -t cifs //192.168.1.100/Compartit /mnt/windows -o username=usuari,password=contrasenya
```  

Per fer-ho permanent, afegim la següent línia a `/etc/fstab`:  
```
//192.168.1.100/Compartit /mnt/windows cifs username=usuari,password=contrasenya 0 0
```  

---

## 4. Autenticació i Gestió d'Usuaris

En entorns empresarials, és fonamental gestionar usuaris de manera centralitzada. Windows utilitza **Active Directory (AD)**, i Linux es pot integrar a AD amb diverses eines.  

### 4.1. Ús de Samba i Winbind per unir Linux a un domini AD

1. **Instal·lar els paquets necessaris**  
   ```bash
   sudo apt install samba winbind libnss-winbind libpam-winbind
   ```  

2. Configurar Samba (`/etc/samba/smb.conf`)  
   ```ini
   [global]
   workgroup = DOMINI
   security = ADS
   realm = EXEMPLE.LOCAL
   idmap config * : backend = tdb
   ```

3. **Afegir el sistema Linux al domini**  
   ```bash
   sudo net ads join -U administrador@EXEMPLE.LOCAL
   sudo systemctl restart winbind
   ```  

4. **Verificar que l'autenticació funciona**  
   ```bash
   wbinfo -u  # Mostra usuaris del domini
   wbinfo -g  # Mostra grups del domini
   ```

---

## 5. Execució d'Aplicacions de Windows a Linux i Viceversa  

### 5.1. Executar Aplicacions de Windows en Linux (Wine i Proton)  
`Wine` permet executar programes de Windows en Linux.  

- Instal·lació:  
  ```bash
  sudo apt install wine
  ```  
- Executar un `.exe`:  
  ```bash
  wine programa.exe
  ```  

Per a compatibilitat millorada amb jocs i aplicacions, `Proton` (de Steam) és una bona alternativa.  

### 5.2. Executar Aplicacions de Linux en Windows (WSL)  
**WSL (Windows Subsystem for Linux)** permet executar Linux dins de Windows sense màquines virtuals.  

- Activació de WSL i instal·lació d’Ubuntu:  
  ```powershell
  wsl --install -d Ubuntu
  ```  
- Un cop instal·lat, es pot executar Linux des del terminal de Windows (`wsl`).

---

## 6. Gestió Remota i Automatització

### 6.1. Connexió Remota entre Windows i Linux 
- **Accedir a Linux des de Windows amb SSH:**  
  ```powershell
  ssh usuari@ip_linux
  ```  
- **Accedir a Windows des de Linux amb RDP:**  
  ```bash
  sudo apt install remmina
  remmina
  ```  

### 6.2. Automatització amb PowerShell i Bash  
- **Executar scripts de PowerShell en Linux:**  
  ```bash
  sudo apt install powershell
  pwsh -File script.ps1
  ```  
- **Usar Ansible per gestionar Windows des de Linux:**  
  ```bash
  ansible windows -m win_ping
  ```

