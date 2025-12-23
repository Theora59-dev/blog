+++
title = "DVWA - Injection SQL"
date = 2025-12-24
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++


- DVWA simule les injections SQL via une requête vulnérable...
  ```SQL
  SELECT first_name, last_name FROM users WHERE user_id = '$id'
  ```
  ... où $id est l'entrée utilisateur non sanitizée, permettant de manipuler la logique SQL pour extraire/dumper la base ou bypass l'authentification.

## Easy

- Aucune protection : entrez `1' UNION SELECT user,password FROM users--` dans le champ `User ID`.
- `UNION` concatène les résultats de la table users (colonnes user, password MD5), `--` commente le reste de la query.
- Résultat : dump des credentials admin (ex. admin:5f4dcc3b5aa765d61d8327deb882cf99).

  {{ responsive_image(src="20251223_160317.png", alt="Responsive hi-res image") }}

## Medium

- Applique `mysql_real_escape_string()` mais sans quotes autour du paramètre (user_id = $id au lieu de = '$id').
- Payload : `1 UNION SELECT user,password FROM users#` (dropdown POST intercepté via Burp, # commente).
- L'escape n'affecte pas les opérateurs/logique hors strings.

## Hard

- Input via popup stocké en $_SESSION['id'], query identique au Low mais indirect.
- Interceptez la requête avec burp, `id=1' UNION SELECT user,password FROM users--`, puis forward.
- Même payload fonctionne car session non sanitizée côté query (Le payload persiste avec la session).
