# Ceux dont on a besoin pour créer l'architecture : 

- BACKEND : possède d'un fichier Dockerfile pour construire son image ainsi que son API. 
- FRONTEND : possède d'un fichier Dockerfile pour construire son image ainsi que les codes reactJS et nodejs

# Ce que nous devons éxécuter dans les deux fichiers Dockerfiles du dossier BACKEND et FRONTEND :

- Premièrement, nous créeons deux dockerfiles dans les dossiers BACKEND et FRONTEND
- Deuxièmement, Nous mettons la première synthaxe avec la commande FROM node:10-alpine (le nom de l'image) pour définir l'image de base
- Troisièmement, Puis on spécifie la commande WORkDIR /usr/src/app pour définir le répertoir de travail pour toutes les instructions
- Quatrièmement, On spécifie la commande COPY package*.json ./ pour copier tous les packages dans le répertoire /
- Cinquièmement, on spécifie la commande RUN npm install pour installer l'image
- Sixièmement, la commande EXPOSE 8080 pour exposer le port particulier. 
- Et à la fin, la commande CMD [ "node", "server.js" ] pour l'exécution de l'image en ligne de commande. 

Voici donc à quoi devrait ressembler notre Dockerfile : 

FROM node:10-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD [ "node", "server.js" ]

