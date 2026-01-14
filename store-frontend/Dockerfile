##Build Angular
#FROM node:20-alpine AS build
#WORKDIR /app
#COPY . .
#RUN npm ci
#COPY . .
#RUN npx ng build --configuration production --output-path=dist/store-frontend --base-href=/new/
#
## Servir con Nginx
#FROM nginx:alpine
#COPY --from=build /app/dist/store-frontend /usr/share/nginx/html
#COPY nginx.conf /etc/nginx/nginx.conf
#COPY conf.d/default.conf /etc/nginx/conf.d/default.conf
#EXPOSE 443

FROM node:20-alpine AS build
WORKDIR /app

COPY store-frontend/package.json store-frontend/package-lock.json ./
RUN npm install
COPY . .
RUN npx ng build --configuration production

# Servir con Nginx
FROM nginx:alpine
# BORRA HTML POR DEFECTO
RUN rm -rf /usr/share/nginx/html/*

# COPIA ANGULAR REAL
COPY --from=build /app/dist/store-frontend /usr/share/nginx/html

#COPY store-frontend/dist/store-frontend /usr/share/nginx/html
COPY store-frontend/nginx.conf /etc/nginx/nginx.conf
COPY store-frontend/conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY store-frontend/certs/fullchain.pem /etc/ssl/fullchain.pem
COPY store-frontend/certs/privkey.pem /etc/ssl/privkey.pem

EXPOSE 443
