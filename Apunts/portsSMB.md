---
output:
  pdf_document: default
  html_document: default
---

### 1. **Identificar els ports utilitzats per Samba**
Els serveis Samba utilitzen aquests ports de manera predeterminada:
- **137 (UDP)**: Servei NetBIOS Name Service
- **138 (UDP)**: Servei NetBIOS Datagram Service
- **139 (TCP)**: Servei NetBIOS Session Service
- **445 (TCP)**: SMB directe (per versions modernes de Samba)

---

### 2. **Configurar el tallafocs (UFW)**

Ubuntu sol utilitzar **UFW** (Uncomplicated Firewall) com a interfície per al tallafocs. Aquí tens com obrir els ports necessaris:

#### Obrir els ports manualment:
Executa aquestes ordres al terminal:
```bash
sudo ufw allow 137/udp
sudo ufw allow 138/udp
sudo ufw allow 139/tcp
sudo ufw allow 445/tcp
```

#### Utilitzar un perfil Samba predefinit:
Samba inclou perfils preconfigurats a **UFW**. Pots habilitar-los així:
```bash
sudo ufw allow Samba
```

Per verificar la configuració del tallafocs:
```bash
sudo ufw status
```

---

### 3. **Comprovar el servei de Samba**
Assegura't que el servei Samba està instal·lat i en funcionament:
```bash
sudo systemctl status smbd
```

Si no està funcionant, pots iniciar-lo amb:
```bash
sudo systemctl start smbd
sudo systemctl enable smbd
```

---

### 4. **Configurar el fitxer `smb.conf`**
El fitxer de configuració de Samba es troba a:
```bash
/etc/samba/smb.conf
```

Assegura’t que les configuracions d’interfície permetin connexions des de la teva xarxa. Per exemple:
```ini
[global]
   workgroup = WORKGROUP
   server string = Samba Server
   netbios name = ubuntu-server
   interfaces = 127.0.0.1 eth0
   bind interfaces only = yes
```

Després de fer canvis, reinicia Samba:
```bash
sudo systemctl restart smbd
```

---

### 5. **Verificar la connectivitat**
Des d'un altre equip de la xarxa, pots fer ping al servidor i comprovar els ports oberts amb comandes com `nmap`:
```bash
nmap -p 137,138,139,445 IP_DEL_SERVIDOR
```
