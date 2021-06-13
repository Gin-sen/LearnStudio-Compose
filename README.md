# Introduction 

Ce projet permet de lancer tous les projets en même temps grâce à `docker-compose`. Il simule une infra avec un reverse-proxy nginx qui s'occupe de rediriger vers les containers définis dans `./nginx/conf/default.conf`.

## Prérequis

- Docker
- Cloner les dépots de manière à ce qu'ils soient tous dans le même dossier comme indiquée ci-dessous
- openssl
- (mode "dev") npm pour `npm install` (version 7.7.6 dans les containers à base node:15.14-alpine)

## Respecter cette disposition !

```txt
Learn-Compose
├── nginx
│   ├── certs
│   └── conf
├── Learn-Database
├── LearnStudio-Back
└── LearnStudio-Front
```

```bash
git clone https://github.com/Gin-sen/Learn-Compose.git
cd Learn-Compose
git clone https://github.com/Gin-sen/Learn-Database.git
git clone https://github.com/Gin-sen/LearnStudio-Back.git
git clone https://github.com/Gin-sen/LearnStudio-Front.git
```


Remarque: **si vous utilisez WSL**, faites en sorte de cloner dans un dossier appartanant a votre hôte wsl (ex : `/home/user_x/pli`) et pas dans un dossier Windows (ex : `/mnt/c/users/user_x/pli`).

## Générer les clés de dev auto-signées

Dans le dossier `Learn-Compose`, entrer :

(linux, mac et wsl)

```bash
mkdir nginx/certs
cd nginx/certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout learn-studio-api.key -out learn-studio-api.crt # Pas besoin de rentrer quoi que ce soit 
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout learn-studio.key -out learn-studio.crt # Pas besoin de rentrer quoi que ce soit 
```

Sur Windows, je sais pas.

## Modifier votre /etc/hosts

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

Lancer le mode prod : `docker-compose -f docker-compose.prod.yml up --build` dans le projet `Learn-Compose`.

Note : docker utilise un système de cache qui évite d'avoir à re-build des parties d'image qui n'ont pas changé, n'hésitez pas sur l'argument `--build` !

Toutes les applications devraient tourner et le nginx reverse proxy devrais servir les containers sur ces urls :

- Frontend : `http://learn-studio.test:8181`
- Backtend : `http://learn-studio-api.test:8181`

TODO: faire une conf nginx pour la redirection d'erreur pour le front (prod uniquement, marche en dev) acr en mode prod, le container angular est just un nginx

## Mode "dev"

Dans ce mode "plus interactif", vos fichiers de code seront directement partagés avec les containers.

Les dépendances des packages sont installées à chaque build, si vous décidez d'ajouter un package, relancez un build.

Les packages du projet `LearnStudio-Back` (api nodejs/express) sont OBLIGATOIRES (voir Troubleshooting) en mode dev, les autres sont optionnels.

Votre IDE aura besoin des packages de vos projets donc n'hésitez pas à installer les packages pour dev dans les dossiers appropriés :
- Pour installer les packages déjà requis pour faire fonctionnrer le projet, utilisez `npm install --save`.
- Pour installer de nouveaux packages, utilisez `npm install --save <package>`.

Lancer le mode dev : `docker-compose up --build` dans le projet `Learn-Compose`.

Toutes les applications devraient tourner et le nginx reverse proxy devrais servir les containers sur ces urls :

- Frontend : `http://learn-studio.test:8181`
- Backtend : `http://learn-studio-api.test:8181`

# Test

Les tests de chaque service seront décris dans les README.md appropriés.


# Troubleshooting

Problèmes déjà connus : 

## Un délire de packages.json et de nodes_modules pour une api nodejs/express

**Mode dev uniquement**

```txt
node_1      | node:internal/modules/cjs/loader:927
node_1      |   throw err;
node_1      |   ^
node_1      |
node_1      | Error: Cannot find module 'express'
node_1      | Require stack:
node_1      | - /app/app.js
node_1      | - /app/bin/www
node_1      |     at Function.Module._resolveFilename (node:internal/modules/cjs/loader:924:15)
...
```

Problème rencontré quand on lance le mode dev sans avoir installé les node_modules


Fix : dans le dossier `./LearnStudio-Back/learn-studio-api` :
- supprimer les nodes_modules s'il y en a 
- supprimer le `package-lock.json`
- Lancer la commande `npm install`

Vous devriez pouvoir lancer le `docker-compose up --build`. Dans le cas contraire, créez une issue dans les Work Items DevOps avec un maximum d'information pour le DevOps.

# Contribute
TODO: Explain how other users and developers can contribute to make your code better. 

## Contribuer au Back/Front/Database

Ce dépot prends en compte les changement pour les fichiers autres que les dépots clonés.

Pour changer de branche dans le back avec votre IDE, ouvrez le projet `LearnStudio-Back` comme un projet indépendant pour que l'IDE prenne le .git de ce projet. (ex: `code LearnStudio-Back` puis changer de branche, faire une modif, commit, ...).