# 4. ern-docker project

After we create a mern stack we have a backend folder for database and a frontend folder. We need to create a Dockerfile into the each other folder.

Dockerfile for frontend folder:

```Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5173
CMD npm run dev
```

the port with `EXPOSE 5173` is the port that we will use to access the frontend and the `CMD npm run dev` is the command that will run when the container is created

Dockerfile for backend folder:

```Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8000
CMD npm start
```
