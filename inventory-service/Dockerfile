FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

FROM node:18-alpine AS runtime

WORKDIR /app 

ENV NODE_ENV=production \
    PORT=3000 \
    PRODUCT_SERVICE_URL="" \
    INVENTORY_SERVICE_URL="" \
    ORDER_SERVICE_URL=""

COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
COPY index.js ./
COPY src ./src

EXPOSE 3001

USER node 

CMD ["node", "index.js"]