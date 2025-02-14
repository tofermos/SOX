---
title: |
  | U9-ADMINISTRACIÓ D'UBUNTU. Monitorització
subtitle: |
  |  Processos
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

Quan envies un procés a **segon pla** (`background`), **continua executant-se**, però ho fa **sense bloquejar la terminal**, permetent-te executar altres ordres al mateix temps.

## Diferències clau:
- **En segon pla (`&`):** El procés està **en execució activa**, però **no ocupa el terminal principal**.  
- **Pausat (`Ctrl + Z`):** El procés està **aturat** fins que li dius `bg` o `fg`.  

## Exemple pràctic per comprovar-ho:
### Envia’l a segon pla:

```bash
sleep 30 &
```
Resultat:
```
[1] 12345
```
- `[1]`: Número de treball (job number).  
- `12345`: PID (Identificador del procés).  

### Comprova si està executant-se amb `jobs`:
```bash
jobs
```
Resultat:
```
[1]+  Running              sleep 30 &
```
- **"Running"**: Sí, està executant-se en segon pla.

---

## Comprova amb `ps` si està executant-se
```bash
ps -ef | grep sleep
```
Veuràs una línia amb el **PID** del procés.

---

## Si el processos s’atura (Stopped)
- Si veus `Stopped` a `jobs`:
```bash
[1]+  Stopped              sleep 30
```
Això passa si l'has aturat amb `Ctrl + Z`.

- Reprèn-lo en segon pla amb:
```bash
bg %1
```

---

## Comprova si s’executa després de tancar el terminal (`nohup`):
```bash
nohup ./el_meu_script.sh &
disown %1
```

*disown* desvincula (sense deixar d'excutar-se) el procés del terminal.

---

## Resum curt
- `&` → Executa i **continua treballant** en segon pla.  
- `jobs` → Verifica si està en execució (`Running` o `Stopped`).  
- `ps` → Mostra processos actius.  
- `bg` → Continua en segon pla si estava pausat.  
- `nohup` → Continua després de tancar la terminal.  
