# Multi-stages Dockerfile

FROM node:lts-alpine AS builder

RUN apk add --no-cache libc6-compat

WORKDIR /app

ENV NODE_ENV=development

COPY package*.json /app
COPY yarn.lock /app

RUN yarn install

COPY . .
RUN yarn build

FROM node:lts-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/node_modules ./node_modules

CMD ["yarn", "start"]
