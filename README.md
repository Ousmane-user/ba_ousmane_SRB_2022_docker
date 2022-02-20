# Ceux dont on a besoin pour créer l'architecture : 

- BACKEND : possède d'un fichier Dockerfile pour construire son image ainsi que son API. 
- FRONTEND : possède d'un fichier Dockerfile pour construire son image ainsi que les codes reactJS et nodejs

# Ce que nous devons éxécuter dans le fichier Dockerfile du dossier BACKEND

- Premièrement, nous créeons un dockerfile dans le dossier BACKEND
- Deuxièmement, Nous mettons la première synthaxe avec la commande FROM node:10-alpine (le nom de l'image) pour définir l'image de base
- Troisièmement, Puis on spécifie la commande WORkDIR /usr/src/app pour définir le répertoir de travail pour toutes les instructions
- Quatrièmement, On spécifie la commande COPY package*.json ./ pour copier tous les packages dans le répertoire /
- Cinquièmement, on spécifie la commande RUN npm install pour installer l'image
- Sixièmement, la commande EXPOSE 8080 pour exposer le port particulier. 
- Et à la fin, la commande CMD [ "node", "server.js" ] pour l'exécution de l'image en ligne de commande. 

Voici donc à quoi devrait ressembler notre Dockerfile du dossier BACKEND : 

FROM node:10-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD [ "node", "server.js" ]


# Ce que nous devons éxécuter dans le fichier Dockerfile du dossier FRONTEND

- Premièrement, nous créeons un dockerfile dans le dossier FRONTEND
- Deuxièmement, Nous mettons la première synthaxe avec la commande FROM node:14-alpine (le nom de l'image) pour définir l'image de base
- Troisièmement, Puis on spécifie la commande WORkDIR /app pour définir le répertoir de travail pour toutes les instructions
- Quatrièmement, On spécifie la commande COPY package*.json ./ pour copier tous les packages dans le répertoire /
- Cinquièmement, on spécifie la commande RUN npm install pour installer l'image
- Sixièmement, la commande EXPOSE 3000 pour exposer le port particulier. 
- Et à la fin, la commande CMD [ "npm", "start" ] pour l'exécution de l'image en ligne de commande. 

Voici donc à quoi devrait ressembler notre Dockerfile du dossier FRONTEND: 

FROM node:14-alpine

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]

# DOCKER-COMPOSE 

Nous allons passer maintenant au docker-compose qui est un fichier qui va nous permettre de lancer notre architecture complète en une seule ligne de commande :

Ceux dont on a besoin pour notre docker-compose : 
- 3 services à spécifier pour la création de notre docker-compose : web, api et mongodb

Tout d'abord, nous créeons un fichier docker-compose.yml, on spécifie la version 3.8 de notre mangobd.
Dans la partie "web", nous spécifions le port "3000:3000". Notre application s'éxécutera sur ce port à l'interieur du conteneur.
Dans la partie "api" dans "build:" nous spécifions la synthaxe ./backend qui va indiquer à Docker de créer l'image API avec l'aide du Dockerfile du repertoire backend.
Le conteneur db est connecté à back (networks), dans notre api et mongodb pourront interagir ensemble. 
Dans la partie mangodb, nous utilisons une image basée sur mongo. La variable environnement: sera utilisé pour l'initialisation du serveur MONGODB avec l'utilisateur et le mot de passe. 
Dans le volume "mangodb_data:/database/db dans le conteneur, les fichiers à l'interieur de ce repertoire s'éxécuteront quand la base de données sera créée.

Voici donc à quoi devrait ressembler notre docker-compose.yml

version: "3.8"
services:
  web:
    build: ./frontend
    depends_on:
      - api
    ports:
      - "3000:3000"
    networks:
      - back


  api:
    build: ./backend
    depends_on:
      - mongo
    ports:
      - "8080:8080"
    networks: 
     - back


  mongo:
    image: mongo
    restart: always
    volumes: 
      - mongodb_data:/database/db
    environment: 
      MONGODB_INITDB_ROOT_USERNAME: ousmane
      MONGODB_INITDB_ROOT_PASSWORD: ousmane

    networks: 
     - back

networks:
  back:

volumes: 
  mongodb_data:

# DANS LA PARTIE POWERSHELL

Nous lançeons la commande "docker-compose up" pour lancer toute l'architecture en une seule fois. 
Pour supprimer l'image, nous lançeons la commande "docker image rm" suivi de "l'image ID" ou plusieurs "images ID" avec la meme commande.
Pour supprimer les conteneurs, nous lançeons la commande "docker-compose down" et pour stopper le conteneur nous lançeons la commande "docker stop".
