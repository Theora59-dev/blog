+++
title = "DVWA - Authorization Bypass"
date = 2025-12-26
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++

L'Authorization Bypass exploite les failles où l'autorisation (permissions utilisateur) n'est pas vérifiée côté serveur, malgré une authentification réussie. DVWA simule des contrôles RBAC (Role-Based Access Control) défaillants.


| Autorisation | Authentification            |
| ------------ | --------------------------- |
| QUI ?        | Oui/Non (Pas très sécurisé) |


## Easy
- Principe : Serveur vérifie authentification (login requis) mais PAS l'autorisation (admin only).
- Exploitation : Naviguez directement vers http://dvwa/security.php (page admin normalement cachée).
- Résultat : Interface complète DVWA Security s'affiche sans restriction, permettant de changer les niveaux Low/Medium/High.

## Medium

- Principe : Cookie DVWA stocke rôle utilisateur (admin/user), non validé à chaque requête.
- Étapes Burp :
  - Login user normal → interceptez requête, notez Cookie: PHPSESSID=xxx; DVWA=user
  - Modifier : DVWA=admin
  - Forward → Accès admin complet (Security, Create/Reset DB)
  - Preuve : Page DVWA Security accessible avec compte basique.

## Hard

- Principe : Erreur 403/Access Denied sur page principale, mais développeur oublie de protéger les endpoints backend/sous-pages.
Tests systématiques :
  ```url
  403 sur /admin.php → tester :
  /admin.php?test=1
  /admin/action.php
  /admin/save.php
  /admin/settings/
  /admin/config
  ```
