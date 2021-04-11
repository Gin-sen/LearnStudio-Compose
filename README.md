# Introduction 

Ce projet permet de lancer tous les projets en même temps grâce à `docker-compose`. Il simule une infra avec un reverse-proxy nginx qui s'occupe de rediriger vers les containers définis dans `./nginx/conf/default.conf`.

## Prérequis

- Docker
- Cloner les dépots de manière à ce qu'ils soient tous dans le même dossier indiquée ci-dessous
- ??maybe openssl si y a besoin de générer des clés quand le https est implémenté??

## Respecter cette disposition !

```txt
.
├── Learn-Compose
├── Learn-Database
├── LearnStudio-Back
└── LearnStudio-Front
```


Remarque: si vous utilisez WSL, faites en sorte de cloner dans un dossier appartanant a votre hôte wsl (ex : `/home/maxime/pli`) et pas dans un dossier Windows (ex : `/mnt/c/users/maxime/pli`).

# Getting Started

1.	Ajoutez dans votre `/etc/hosts` (linux)

```txt
127.0.0.1 learn-studio.test
127.0.0.1 learn-studio-api.test
```
2.	Run `docker-compose up --build` at the root of the `Learn-Compose` project.

All of your apps should be running and the nginx reverse proxy should serve the apps like so :

- Frontend : `http://learn-studio.test:8080`
- Backtend : `http://learn-studio-api.test:8080`

# Test

Les tests de chaque service seront décris dans les README.md appropriés.


# Contribute
TODO: Explain how other users and developers can contribute to make your code better. 
