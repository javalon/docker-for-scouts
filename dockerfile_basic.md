# Basic dockefile

FROM node:lts-alpine

RUN apk add --no-cache libc6-compat

WORKDIR /app

COPY package*.json /app
COPY yarn.lock /app

RUN yarn install

COPY . .

RUN yarn build

ENV NODE_ENV=production

CMD ["yarn", "start"]
