+++
title = "DVWA - XSS (STORED)"
description = "Explication technique"

date = 2025-12-23
categories = ["DVWA"]



[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++

## Low

> Voici un apercu de l'application vulnérable
{{ responsive_image(src="illustration1.png", alt="Image d'illustration") }}

- Au niveau Easy, aucune protection n'empêche l'injection de code JavaScript. Entrez simplement `<script>alert('Something')</script>` dans le champ "Message" du guestbook et soumettez.
{{ responsive_image(src="illustration2.png", alt="Image d'illustration") }}

## Medium

- DVWA filtre la chaîne "script" en minuscules via str_replace, mais ignore les variantes de casse.
- Utilisez `<ScRiPt>alert('XSS Stored Medium!')</ScRiPt>` ou `<SCRIPT>alert('XSS')</SCRIPT>` pour contourner le remplacement partiel.

## Hard
- L'application protège le champs "Message" suffisamment, et le champ "Name" limite la quantitée de caractère entré. 
- Il faut donc utiliser un proxy (Burp) pour auditer correctement l'applicative.
{{ responsive_image(src="illustration3.png", alt="Image d'illustration") }}
- Et ça fonctionne !!
{{ responsive_image(src="illustration4.png", alt="Image d'illustration") }}
