Per configurar **LDAP** en una LAN **sense TLS** ni certificats, la configuració és més senzilla. El trànsit no estarà xifrat, però funcionant dins d'una xarxa local (LAN), és una configuració acceptada si la seguretat no és una preocupació crítica en aquest cas.

A continuació et dono les indicacions per configurar **LDAP** sense TLS ni certificats en una LAN.

---

### **1. Configuració del fitxer `/etc/nslcd.conf` en el client (Lubuntu)**

Aquí tens el fitxer **`/etc/nslcd.conf`** configurat per una connexió LDAP sense TLS ni certificats. Això utilitza **`ldap://`** (no **`ldaps://`**) per la connexió no segura (port 389) al servidor LDAP:

```plaintext
# /etc/nslcd.conf
# nslcd configuration file. See nslcd.conf(5) for details.

# The user and group nslcd should run as.
uid nslcd
gid nslcd

# The location at which the LDAP server(s) should be reachable.
uri ldap://192.168.1.1

# The search base that will be used for all queries.
base dc=smx2b,dc=com

# The LDAP protocol version to use.
ldap_version 3

# The DN to bind with for normal lookups.
binddn cn=admin,dc=smx2b,dc=com
bindpw Gandia2425

# SSL options (disable TLS/SSL for LAN setup)
ssl off
tls_reqcert never

# The search scope.
scope sub
```

### **Explicació dels canvis**

1. **`uri ldap://192.168.1.1`**:
   - Especifica el protocol **`ldap://`** (sense TLS, utilitzant el port 389). Això és perquè no estàs utilitzant una connexió segura.

2. **`ssl off`**:
   - Desactiva el suport per TLS/SSL, ja que no estàs utilitzant connexions xifrades.

3. **`tls_reqcert never`**:
   - Desactiva la verificació de certificats, ja que no estàs utilitzant certificats SSL/TLS.

4. **`ldap_version 3`**:
   - Configura la versió del protocol LDAP a **v3**, que és l'estàndard actual per LDAP.

5. **`scope sub`**:
   - Estableix el **scope** de cerca a **`sub`**, per incloure subtrees complets dins de la base DN especificada. Aquesta és la configuració per defecte, així que no és estrictament necessari, però la pots mantenir explícitament.

6. **`binddn` i `bindpw`**:
   - **`binddn`** especifica el DN amb el qual el client es connectarà al servidor LDAP. En aquest cas, s'utilitza un compte d'administrador (com per exemple `cn=admin,dc=smx2b,dc=com`).
   - **`bindpw`** és la contrasenya associada amb aquest DN (en el teu cas **Gandia2425**).

---

### **2. Instal·lacions necessàries al client**

1. **Instal·la les eines necessàries per LDAP**:
   Si no ho has fet encara, assegura't d'instal·lar els paquets necessaris per treballar amb LDAP al client:

   ```bash
   sudo apt update
   sudo apt install libnss-ldap libpam-ldap ldap-utils nslcd
   ```

2. **Verifica que el paquet `ca-certificates` estigui instal·lat**:
   Tot i que no utilitzem TLS ni certificats, és una bona pràctica tenir el paquet `ca-certificates` instal·lat per assegurar que els sistemes que utilitzin certificats en el futur es gestionin correctament.

   ```bash
   sudo apt install ca-certificates
   ```

---

### **3. Configuració de **PAM** i **NSS** al client**

Per permetre la integració amb LDAP per a autenticació i cercar usuaris del directori LDAP, cal que configuris adequadament els fitxers **PAM** i **NSS**.

1. **Modifica el fitxer `/etc/nsswitch.conf`**:

   Afegeix **`ldap`** a les línies que gestionen la informació d'usuaris i grups:

   ```bash
   sudo nano /etc/nsswitch.conf
   ```

   Modifica aquestes línies:

   ```plaintext
   passwd:     files ldap
   group:      files ldap
   shadow:     files ldap
   ```

2. **Configura PAM per a LDAP**:

   Això permetrà l'autenticació d'usuaris LDAP.

   1. **Autenticació (`common-auth`)**:

      ```bash
      sudo nano /etc/pam.d/common-auth
      ```

      Afegeix la línia següent per permetre l'autenticació mitjançant LDAP:

      ```plaintext
      auth    sufficient      pam_ldap.so
      ```

   2. **Compte (`common-account`)**:

      ```bash
      sudo nano /etc/pam.d/common-account
      ```

      Afegeix aquesta línia:

      ```plaintext
      account sufficient      pam_ldap.so
      ```

   3. **Sessió (`common-session`)**:

      Per crear directoris d'inici automàticament per usuaris LDAP que inicien sessió:

      ```bash
      sudo nano /etc/pam.d/common-session
      ```

      Afegeix aquesta línia:

      ```plaintext
      session required        pam_mkhomedir.so skel=/etc/skel umask=0022
      ```

---

### **4. Comprovar la configuració**

1. **Prova la cerca LDAP**:
   Comprova que el client pot consultar el directori LDAP executant una cerca simple:

   ```bash
   getent passwd
   getent group
   ```

   Això hauria de retornar els usuaris i grups definits al servidor LDAP.

2. **Prova d'autenticació**:
   Prova d'iniciar sessió amb un compte definit al servidor LDAP. Si tot està configurat correctament, hauries de poder autenticar-te utilitzant un usuari LDAP.

---

### **5. Configuració del servidor LDAP (si és necessari)**

Assegura't que el servidor LDAP està configurat per acceptar connexions no segures (sense TLS), normalment a través del port 389.

- Si utilitzes **OpenLDAP**, comprova que el servidor estigui configurat per escoltar al port 389:
  - Si utilitzes **`slapd.conf`**: La línia per definir el port serà:
    ```plaintext
    ldap:///
    ```
  - Si utilitzes **`cn=config`**: Hauràs de configurar el port per LDAP al fitxer de configuració dinàmic.

Reinicia el servei LDAP després de qualsevol canvi:

```bash
sudo systemctl restart slapd
```

---

### **Consideracions finals**
- Aquesta configuració **no utilitza connexions segures** (sense TLS), així que **les credencials d'usuari viatjaran en text pla**. Aquesta és una configuració que podria ser adequada per a un entorn de prova o una LAN tancada on la seguretat de la xarxa no sigui una preocupació crítica.
- Si més endavant decideixes millorar la seguretat i utilitzar TLS, els passos que has de seguir per configurar-lo s'han detallat anteriorment.

Amb aquests passos, tindràs un client Lubuntu que es connecta al servidor OpenLDAP a través d'una connexió no segura en una LAN.