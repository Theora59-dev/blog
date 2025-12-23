+++
title = "DVWA - Command Injection"
date = 2025-12-21
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++

## Easy:
Au niveau Easy, aucune validation n'empêche l'injection directe. La commande PHP exécute ping IP sans filtrage, permettant d'ajouter une seconde commande via des séparateurs shell comme `;`, `&&` ou `||`.

- Entrez `127.0.0.1; whoami` pour exécuter `ping 127.0.0.1` suivi de `whoami`, affichant l'utilisateur du serveur.
- `127.0.0.1 && ls` fonctionne si la première commande réussit, listant les fichiers.
- Ces séparateurs chainent les commandes sans restriction.

## Medium:
DVWA applique un blacklistage basique via `str_replace`, supprimant `;`, `&&` et `||` en remplaçant par des espaces vides, rendant les injections classiques inefficaces
- Testez `127.0.0.1 || whoami` : le double pipe `||` exécute la seconde commande seulement si la première échoue, contournant partiellement le filtre car `||` n'est pas toujours bloqué immédiatement.
- Si `||` est filtré, essayez le pipe simple `|` pour forcer l'exécution conditionnelle.
- Une IP invalide comme `999.999.999.999 || id` déclenche l'échec du `ping`, exécutant `id`.

## High:
Les filtres s'intensifient : espaces interdits, whitespaces remplacés, et extension de la blacklist aux séparateurs courants, bloquant les injections en une ligne.


- Solution experimentale: Faire passer la requête par un Proxy (BurpSuite) et insérer un saut de ligne dans le champ vulnérable 
{{ responsive_image(src="Command Injection illustration.png", alt="Responsive hi-res image") }}
