# ---- Étape 1 : construire l'application Angular ----
FROM node:22-alpine AS build
WORKDIR /app
# Installer les dépendances
COPY package*.json ./
RUN npm ci
# Copier le code et générer la version de production
COPY . .
RUN npm run build

# ---- Étape 2 : servir les fichiers avec Nginx ----
FROM nginx:alpine
# Configuration Nginx (routes Angular)
COPY nginx.conf /etc/nginx/conf.d/default.conf
# Copier le résultat du build Angular
COPY --from=build /app/dist/frontend/browser /usr/share/nginx/html
EXPOSE 80
