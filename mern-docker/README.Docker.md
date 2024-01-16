# 4. Mern-docker project

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
we need to modify in the every directories the package.json file with the following: **(--host)**

```json
"scripts": {
    "dev": "vite --host",
    ...
    ...
}
```

After all we need create a compose.yaml into the root folder to specify the web services and the database (web, api, db, folders).

```yaml
version: "3.8"

services:
  web:
    depends_on:
      - api
    build: ./frontend
    ports:
      - 5173:5173
    environment:
      VITE_API_URL: http://localhost:8000
    develop:
      watch:
        - path: ./frontend/package.json
          action: rebuild
        - path: ./frontend/package-lock.json
          action: rebuild
        - path: ./frontend
          target: /app
          action: sync

  api:
    depends_on:
      - db
    build: ./backend
    ports:
      - 8000:8000
    environment:
      DB_URL: mongodb://db/anime
    develop:
      watch:
        - path: ./backend/package.json
          action: rebuild
        - path: ./backend/package-lock.json
          action: rebuild
        - path: ./backend
          target: /app
          action: sync

  db:
    image: mongo:latest
    ports:
      - 27017:27017
    volumes:
      - anime:/data/db

volumes:
  anime:
```

We can run the command `docker-compose up` to run the services.

```shell
cd mern-docker
docker compose up
```

This will create four docker images and attaching together.
docker-compose up

This command runs a `Docker Compose` file to start and configure multiple Docker containers defined in that file.

It takes as input a `compose.yaml` file which specifies the different services or containers to be created. This file defines each service's Dockerfile build instructions, ports to expose, dependencies between services, environment variables, volumes to mount, etc.

The output is that it builds the Docker images for each service if needed, creates containers from those images, attaches their logging output to the terminal, and starts the containers up.

The `compose.yaml` file allows defining multiple services that can link together, for example a web frontend, API backend, and database. The docker-compose up command handles assembling these structured services defined in the YAML file, building the images, starting the containers, and linking them together.

It achieves this by reading the YAML file and executing Docker commands like docker build, docker create, docker start, etc under the hood to build and configure each service. It links services together by connecting specified ports and setting environment variables between the containers.

The key logic flows are:

- Parse compose.yaml file

- Build Docker images for each service if needed

- Create containers from images for each service

- Link service containers together if dependencies defined

- Start all containers

- Attach container logs to terminal

- Monitor containers and restart as needed per the YAML config

- So in summary, the `docker compose up` command simplifies running multi-container Docker applications by automating the process of building, starting, and linking containers for the services defined in a docker-compose.yml file

Eventually we can see the frontend and backend running in the browser on the http://localhost:5173.
