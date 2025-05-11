# FROM node:21
# WORKDIR /app 
# COPY package*.json ./
# RUN npm install
# COPY . .
# EXPOSE 5173
# RUN npm start
# OR
FROM node:21 AS frontend-build
WORKDIR /app 
COPY package*.json ./
RUN npm install
COPY . .

FROM node:23-slim
WORKDIR /app
COPY --from=frontend-build /app .
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host"]   


