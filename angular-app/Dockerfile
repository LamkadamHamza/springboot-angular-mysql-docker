FROM node:18.17.1-alpine AS build

WORKDIR /app
COPY package*.json ./
RUN npm ci
RUN npm install -g @angular/cli
COPY . .
RUN npm run build --force

### STAGE 2: Run ###
FROM nginx:stable
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist/angular-app/browser /usr/share/nginx/html
EXPOSE 80
