FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

FROM node:18-alpine AS runtime

WORKDIR /app 

ENV NODE_ENV=production

COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
COPY index.js ./
COPY src ./src

EXPOSE 3003

USER node 

CMD ["node", "index.js"]