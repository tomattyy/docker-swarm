
FROM node:24-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm prune --production

FROM node:24-alpine AS runtime

WORKDIR /app 

ENV NODE_ENV=production \
    PORT=3002 \
    DB_HOST="" \
    DB_PORT=5432 \
    DB_NAME="" \
    DB_USER="" \
    DB_PASSWORD=""

RUN addgroup -S admin && adduser -S admin -G admin
COPY --from=builder --chown=admin:admin /app/node_modules ./node_modules
COPY --from=builder --chown=admin:admin /app/src ./src
COPY --from=builder --chown=admin:admin /app/index.js ./index.js
COPY --from=builder --chown=admin:admin /app/package.json ./

USER admin

EXPOSE 3002

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD node -e "require('http').get('http://localhost:'+process.env.PORT+'/health',r=>process.exit(r.statusCode===200?0:1)).on('error',()=>process.exit(1))"

CMD ["node", "index.js"]