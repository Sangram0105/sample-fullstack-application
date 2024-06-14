
FROM node:alpine as build

WORKDIR /app


COPY package.json package-lock.json ./


RUN npm install

COPY . .


ENV NEXT_PUBLIC_BACKEND_BASE_URL=http://localhost:5000


RUN npm run build


FROM node:alpine


WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=build /app/package.json /app/package-lock.json ./
COPY --from=build /app/.next ./.next
COPY --from=build /app/public ./public


RUN npm install --production


EXPOSE 3000


CMD ["npm", "start"]
