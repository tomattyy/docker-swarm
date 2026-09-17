FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

FROM node:20-alpine AS runtime

WORKDIR /app 

ENV NODE_ENV=production

EXPOSE 3002

USER node 

CMD ["node", "index.js"]