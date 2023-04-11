FROM node:18-alpine

RUN apk update
RUN apk add xdg-utils
RUN apk add xvfb
RUN npm install --location=global vim
RUN npm install --location=global npm
RUN npm install --location=global jest -y
RUN npm install --location=global semistandard -y
