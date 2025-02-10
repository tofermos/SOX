---
title: |
  | U9-PROGRAMACIÓ DE TASQUES (at i cron)
subtitle: |
  |  Programar tasques. **at i cron**
Author: "Tomàs Ferrandis Moscardó"
output:
  html_document:
    toc: true
    toc_float: true
    toc_depth: 2
    df_print: paged
    number_sections: false
  pdf_document: 
    toc: false
    keep_tex: true
  word_document:
    toc: true
    toc_depth: '3'
lang: "es-ES"
fontsize: 12 pt
mainfont: Times New Roman
papersize: A4
linestretch: 1.5
---

# 1. ÚS D'`at`
L'ordre `at` permet programar tasques que s'executaran **una sola vegada** en el futur.

## 1.1 Activació del servei `atd`

```bash
sudo systemctl start atd
sudo systemctl enable atd
```

### Exemple 1: Crear una tasca amb `at`

1. Escriu una ordre que vols executar a una hora específica:

```bash
echo "echo 'Hola, món!' >> ~/hola.txt" | at 14:00
```

```bash
at 14:00 -f backup.sh
```

```bash
sudo apt update && sudo apt upgrade -y
```



Això escriurà "Hola, món!" al fitxer `hola.txt` a les **14:00**.

2. També pots utilitzar formats de temps relatius:

```bash
echo "sudo apt update" | at now + 2 minutes
```
Això executarà l'ordre **`sudo apt update`** d'ací a 2 minuts.

## 1.2 Llistar tasques programades

```bash
atq
```

## 1.3 Eliminar una tasca programada

1. Troba l'ID de la tasca amb `atq`.  

2. Elimina-la amb:

```bash
atrm <id>
```

---

# 2. ÚS DE CRON `cron`
`cron` és ideal per programar **tasques recurrents**.

## 2.1 Configuració del cron

1. Obriu l'editor del cron:

```bash
crontab -e
```

2. Afegiu una línia per programar una tasca. Per exemple:

```
0 6 * * * echo "Bon dia!" >> ~/bon_dia.txt
```
Això escriurà "Bon dia!" al fitxer `bon_dia.txt` cada dia a les **6:00 AM**.

## 2.2 Sintaxi de cron

- **Minut (0-59)**  

- **Hora (0-23)**  

- **Dia del mes (1-31)**  

- **Mes (1-12)**  

- **Dia de la setmana (0-7)** (on 0 i 7 són diumenge)

Exemples:  

1. Executar una tasca cada 5 minuts:

```
*/5 * * * * /path/to/script.sh
```

2. Programar una tasca el primer dia de cada mes a les 12 del migdia:

```
0 12 1 * * /path/to/script.sh
```

3. Reiniciar el sistema cada diumenge a mitjanit:

```
0 0 * * 0 sudo reboot
```

## 2.3 Llistar tasques

Per veure les tasques configurades:

```bash
crontab -l
```

## 2.4 Eliminar tasca

Per eliminar totes les tasques del cron:

```bash
crontab -r
```

---

# 3. DIFERÈNCIES ENTRE `at` i `cron`

| **`at`**                 | **`cron`**               |
|--------------------------|--------------------------|
| Execució **una sola vegada** | Execució **recurrent** |
| Ordres simples i puntuals  | Ideal per programacions periòdiques |
| Requereix el servei `atd` | Requereix el dimoni `cron` |

