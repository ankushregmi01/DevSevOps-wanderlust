
FROM node:21 AS backend-builder
WORKDIR /app
COPY . .
RUN npm install
RUN npm run test
FROM node:21-slim
WORKDIR /app
COPY --from=backend-builder /app .
EXPOSE 8080
CMD ["npm", "start"]