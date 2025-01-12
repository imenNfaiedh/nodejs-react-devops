# Utilisez une image de base Node.js pour la construction
FROM node:14 as build

# Répertoire de travail
WORKDIR /app

# Copiez les fichiers package.json et package-lock.json
COPY package*.json ./

# Installez les dépendances
RUN npm install

# Copiez le reste de l'application
COPY . .

# Construisez l'application
RUN npm run build

# Utilisez une image de base Nginx pour servir l'application
FROM nginx:alpine

# Copiez les fichiers de build dans le dossier Nginx
COPY --from=build /app/build /usr/share/nginx/html

# Exposez le port 80
EXPOSE 80

# Commande pour démarrer Nginx
CMD ["nginx", "-g", "daemon off;"]
