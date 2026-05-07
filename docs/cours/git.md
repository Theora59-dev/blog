> Voici une cheatsheet concise sur l'utilisation de Git, centrée sur les commits, la gestion des branches, les fusions et le rebase. 

## Initialisation
- `git init` : Initialise un nouveau dépôt Git dans le répertoire courant, créant le dossier .git pour stocker l'historique et la configuration. Utile pour démarrer un projet local.

- `git clone <URL>` : Crée une copie locale complète d'un dépôt distant, y compris tout l'historique. Idéal pour récupérer un projet existant.

## État et Historique
- `git status` : Affiche l'état actuel du dépôt : fichiers modifiés, ajoutés au staging (index) ou prêts à commiter.

- `git log` Montre l'historique des commits (auteur, date, message).
- `git log --oneline` : Version compacte (un commit par ligne).
- `git log -n 3` : Limite aux 3 derniers commits.
- `git log --graph` : Vue graphique des branches.

## Staging et Commits
- `git add <fichier>` ou `git add .` : Ajoute les modifications au staging area (index), préparant les fichiers pour un commit. `git add .` ajoute tout - utile pour sélectionner précisément ce qui sera validé.
- `git commit -m "Message"` : Valide les changements du staging avec un message descriptif, créant un snapshot permanent dans l'historique. Le message explique "quoi" et "pourquoi".
- `git commit --amend` : Modifie le dernier commit (ajoute des fichiers oubliés ou corrige le message). Utile pour des ajustements rapides sans polluer l'historique.

## Branches
- `git branch` : Liste les branches locales.
- `git branch -a` : Inclut les branches locales et distantes (après git fetch).
- `git branch <nom>` : Crée une branche sans changer.
- `git branch -d <nom>` : Supprime une branche fusionnée (sûr).
- `git checkout <branche>` : Bascule vers une branche existante, mettant à jour les fichiers de travail.
- `git checkout -b <nom>` : Crée et bascule sur une nouvelle branche (moderne : `git switch -c <nom>`). Permet d'isoler des développements sans affecter la branche principale.

## Fusion (Merge)
- `git merge <branche>` : Intègre les commits d'une branche dans la branche courante, créant un commit de fusion si divergence. Utile pour combiner des features terminées (gère les conflits manuellement).
- `git merge --abort` : Annule une fusion en cours (ex. : conflit non résolu). Sauvegarder en cas de problème.
Rebase
- `git rebase <branche>` : Déplace les commits de la branche courante "au-dessus" de la cible, créant un historique linéaire (pas de commit de fusion). Idéal pour un historique propre avant de merger sur main.
- `git rebase -i HEAD~3` : Rebase interactif sur les 3 derniers commits : réorganise, édite, squash ou drop. Puissant pour nettoyer l'historique.
- `git rebase --abort` / `git rebase --continue` : Annule ou reprend un rebase après résolution de conflits. Gère les interruptions.

## Divers: 
- `git diff` : Voir les changements non stagés. `git diff --staged` : Différences prêtes à commit.
- `git stash` : Sauvegarde temporaire des changements non commit. `git stash apply` : Les restaures.
