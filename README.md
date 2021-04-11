# Introduction 

Ce projet permet de lancer tous les projets en même temps grâce à `docker-compose`. Il simule une infra avec un reverse-proxy nginx qui s'occupe de rediriger vers les containers définis dans `./nginx/conf/default.conf`.

## Prérequis

- Docker
- Cloner les dépots de manière à ce qu'ils soient tous dans le même dossier indiquée ci-dessous
- openssl

## Respecter cette disposition !

```txt
pli-wsl/
├── Learn-Compose
├── Learn-Database
├── LearnStudio-Back
└── LearnStudio-Front
```


Remarque: **si vous utilisez WSL**, faites en sorte de cloner dans un dossier appartanant a votre hôte wsl (ex : `/home/maxime/pli`) et pas dans un dossier Windows (ex : `/mnt/c/users/maxime/pli`).

## Générer les clés

Dans le dossier `Learn-Compose`, entrer :

```bash
mkdir nginx/certs
cd nginx/certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout learn-studio-api.key -out learn-studio-api.crt
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout learn-studio.key -out learn-studio.crt
```

# Getting Started

1.	Ajoutez dans votre `/etc/hosts` (linux et mac) ou dans `C:\Windows\System32\drivers\etc\hosts` (Windows et wsl) 

```txt
127.0.0.1 learn-studio.test
127.0.0.1 learn-studio-api.test
```

Pour faire ca en powershell en mode admin : 
```ps1
Start notepad++ C:\Windows\System32\drivers\etc\hosts
```

2.	Lancer `docker-compose up --build` dans le projet `Learn-Compose`.

Toutes les applications devraient tourner et la nginx reverse proxy devrais servir les containers sur ces urls :

- Frontend : `http://learn-studio.test:8080`
- Backtend : `http://learn-studio-api.test:8080`

# Test

Les tests de chaque service seront décris dans les README.md appropriés.


# Contribute
TODO: Explain how other users and developers can contribute to make your code better. 
