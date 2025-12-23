+++
title = "DVWA - XSS (REFLECTED)"
date = 2025-12-23
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++

## Easy
- Aucune sanitisation : entrez `<script>alert('XSS Reflected Easy!')</script>` dans le champ "Name".
- La page reflète immédiatement le payload et exécute l'alert au chargement, confirmant l'absence de protections.

## Medium
- Filtre `str_replace('script', '', $name)` sensible à la casse sur minuscules.
- Utilisez `<sCriPt>alert('XSS Medium!')</sCriPt>` ou `<SCRIPT>alert('XSS')</SCRIPT>` pour éviter le remplacement.
- Le payload s'exécute car le filtre ignore les majuscules, typique des blacklists naïves.

## Hard
- Utilise `htmlspecialchars()` + blacklist "script" insensible à la casse (stripos).
- Injectez `<img src=x onerror=alert('XSS High!')>` ou `"><script>alert('XSS')</script>` pour fermer l'attribut value et ouvrir un nouveau contexte.
- L'événement `onerror` déclenche JS sur image invalide ; testez aussi `<svg onload=alert('High')>` pour polyvalence.
