
FROM node:20.11.1-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm cache clean --force && npm install --legacy-peer-deps

COPY . .

RUN npm run lint

RUN npm run test

RUN npx prisma generate

RUN npm run build


FROM node:20.11.1-alpine

WORKDIR /app

COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/prisma ./prisma

EXPOSE 3000

CMD ["npm", "run", "start:migrate:prod"]
