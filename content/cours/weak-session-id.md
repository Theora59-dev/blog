+++
title = "DVWA - ID de session vulnérable"
date = 2025-12-23
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++

> Utilisez L'extension firefox "Cookie Editor" pour faciliter l'édition des cookie de session (où autre)

## Easy

- Les cookies de session suivent une suite logique simple, incrémentée à chaque nouvelle session.
- Ouvrez deux onglets : notez le cookie "PHPSESSID" du premier (ex. : 1), créez-en un second pour obtenir 2, puis prédisez 3 pour hijacker une session admin.
- Le cookie change toutes les quelques secondes lors de la création de session, sans randomisation.

## Medium

- Le cookie utilise un timestamp epoch (secondes depuis le 1er janvier 1970 UTC) comme base.
- Générez une session, extrayez le cookie (ex. : 1703350000), attendez 10 secondes pour le suivant, ou calculez manuellement en ajoutant l'intervalle observé (souvent 1-10s).
- Hijackez en injectant le cookie prédit dans un navigateur pour accéder à une session valide.

## High

- Le cookie combine timestamp + élément faible, hashé (souvent MD5 ou SHA1 détectable).
- Analysez plusieurs cookies : divisez par epoch pour isoler le timestamp, testez le préfixe/suffixe fixe, puis générez le hash manuellement ou via outils comme CrackStation.net pour les hashes courts.
- Exemple : si le hash est équivalent à MD5(timestamp) ou MD5(smth), essayez de prédire pour le temps courant et injectez pour usurper la session.
