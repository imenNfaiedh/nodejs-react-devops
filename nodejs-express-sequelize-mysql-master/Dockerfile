# Utilisez une image de base Node.js
FROM node:14

# Créez un répertoire de travail
WORKDIR /app

# Copiez les fichiers package.json et package-lock.json
COPY package*.json ./

# Installez les dépendances
RUN npm install

# Copiez le reste de l'application
COPY . .

# Exposez le port de l'application
EXPOSE 8080

# Commande pour démarrer l'application
CMD ["npm", "start"]
