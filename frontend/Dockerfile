FROM node:current-alpine
WORKDIR /usr/src/app

COPY package*.json .
COPY svelte.config.js .
COPY tsconfig.json .
COPY .npmrc .
#COPY /src .
#COPY /scripts .

RUN npm install