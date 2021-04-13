# Introduction 

Ce projet permet de lancer tous les projets en même temps grâce à `docker-compose`. Il simule une infra avec un reverse-proxy nginx qui s'occupe de rediriger vers les containers définis dans `./nginx/conf/default.conf`.

## Prérequis

- Docker
- Cloner les dépots de manière à ce qu'ils soient tous dans le même dossier comme indiquée ci-dessous
- openssl
- (mode "dev") npm 15.14 pour `npm install`

## Respecter cette disposition !

```txt
.
├── Learn-Compose
├── Learn-Database
├── LearnStudio-Back
└── LearnStudio-Front
```


Remarque: **si vous utilisez WSL**, faites en sorte de cloner dans un dossier appartanant a votre hôte wsl (ex : `/home/user_x/pli`) et pas dans un dossier Windows (ex : `/mnt/c/users/user_x/pli`).

## Générer les clés

Dans le dossier `Learn-Compose`, entrer :

```bash
mkdir nginx/certs
cd nginx/certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout learn-studio-api.key -out learn-studio-api.crt
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout learn-studio.key -out learn-studio.crt
```

Ajoutez dans votre `/etc/hosts` (linux et mac) ou dans `C:\Windows\System32\drivers\etc\hosts` (Windows et wsl) 

```txt
127.0.0.1 learn-studio.test
127.0.0.1 learn-studio-api.test
```

Pour ouvrir NotePad++ et faire ça en powershell (powershell à ouvrir en mode admin) : 
```ps1
Start notepad++ C:\Windows\System32\drivers\etc\hosts
```
# Lancer le projet

## Mode "prod"

Lancer le projet dans ce mode correspond à lancer tout l'infra de manière isolé, . Les containers copient le code/fichiers de conf et n'ont plus d'interraction avec l'hôte. Les modifications seront présentes en re-buildant les images docker.

Il est important de noter que vous n'avez pas impérativement besoin d'installer les packages des projets pour lancer ce mode, les containers sont autonomes vis-à-vis de ça. (voir différences entre les volumes des deux fichiers `docker-compose*.yml` + Dockerfiles respectives + .dockerignore respectifs)

**MAIS** votre IDE préferera avoir vos packages pour pouvoir vous suggérer des choses et installer un nouveau package avec npm impliquera sans doutes l'installation de tous les packages, si vous êtes en train de faire des modifications majeures, veuillez lancer le mode "dev".

Lancer `docker-compose -f docker-compose.prod.yml up --build` dans le projet `Learn-Compose`.

Note : docker utilise un système de cache qui évite d'avoir à re-build des parties d'image qui n'ont pas changé, n'hésitez pas sur l'argument `--build` !

Toutes les applications devraient tourner et le nginx reverse proxy devrais servir les containers sur ces urls :

- Frontend : `http://learn-studio.test:8080`
- Backtend : `http://learn-studio-api.test:8080`

TODO: faire une conf nginx pour la redirection d'erreur pour le front (prod uniquement, marche en dev) acr en mode prod, le container angular est just un nginx

## Mode "dev"

Dans ce mode "plus interactif", vos fichiers de code seront directement partagés avec les containers.

Les dépendances des packages sont installées à chaque build, si vous décidez d'ajouter un package, relancez un build.
Pour installer de nouveaux packages, utilisez `npm install <package>` comme d'habitude.

Votre IDE aura besoin des packages de vos projets donc n'hésitez pas a installer les packages pour dev.

Lancer `docker-compose up --build` dans le projet `Learn-Compose`.

Toutes les applications devraient tourner et le nginx reverse proxy devrais servir les containers sur ces urls :

- Frontend : `http://learn-studio.test:8080`
- Backtend : `http://learn-studio-api.test:8080`

# Test

Les tests de chaque service seront décris dans les README.md appropriés.

# Contribute
TODO: Explain how other users and developers can contribute to make your code better. 
