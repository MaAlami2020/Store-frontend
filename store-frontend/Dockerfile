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

COPY store-frontend/*.json ./
COPY store-frontend/*.ts ./
COPY store-frontend/src/*.ts ./
RUN npm install
COPY store-frontend/ ./
RUN chmod +x node_modules/.bin/ng
RUN npx ng build --configuration production --base-href=/spa/

RUN ls -l /app/dist/store-frontend

# Servir con Nginx
FROM nginx:alpine


# COPIA ANGULAR REAL
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/dist/store-frontend/browser /usr/share/nginx/html/
COPY --from=build /app/dist/store-frontend/server /usr/share/nginx/html/
COPY --from=build /app/dist/store-frontend/prerendered-routes.json /usr/share/nginx/html/
COPY --from=build /app/dist/store-frontend/3rdpartylicenses.tx /usr/share/nginx/html/

#COPY store-frontend/dist/store-frontend /usr/share/nginx/html
#COPY store-frontend/nginx.conf /etc/nginx/nginx.conf
COPY store-frontend/conf.d/default.conf /etc/nginx/conf.d/default.conf
COPY store-frontend/certs/fullchain.pem /etc/ssl/fullchain.pem
COPY store-frontend/certs/privkey.pem /etc/ssl/privkey.pem

EXPOSE 443
