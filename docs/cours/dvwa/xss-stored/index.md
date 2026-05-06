## Low

> Voici un apercu de l'application vulnérable
![]("illustration1.png")

- Au niveau Easy, aucune protection n'empêche l'injection de code JavaScript. Entrez simplement `<script>alert('Something')</script>` dans le champ "Message" du guestbook et soumettez.
![]("illustration2.png")

## Medium

- DVWA filtre la chaîne "script" en minuscules via str_replace, mais ignore les variantes de casse.
- Utilisez `<ScRiPt>alert('XSS Stored Medium!')</ScRiPt>` ou `<SCRIPT>alert('XSS')</SCRIPT>` pour contourner le remplacement partiel.

## Hard
- L'application protège le champs "Message" suffisamment, et le champ "Name" limite la quantitée de caractère entré. 
- Il faut donc utiliser un proxy (Burp) pour auditer correctement l'applicative.
![]("illustration3.png")
- Et ça fonctionne !!
![]("illustration4.png")
