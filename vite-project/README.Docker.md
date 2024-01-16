# 3. Vite-docker project

create a vite project

```shell
npm create vite@latest vite-project
```

select React --> TypeScript after go into the folder

```shell
cd vite-project
```

create a Docker setting files with the following:

```shell
docker init
```

Answers:

- What application platform does your project use? --> NPM
-
- What version of Node do you want to use? --> press return
-
- Which package manager do you want to use? --> npm
-
- Do you want to run "npm run build" before starting your server? --> n
-
- What command do you want to start to app? --> npm run dev
-
- What port does your server listen on? --> 5173

We need to edit the compose.yaml file with the following:

```yaml
services:
  web:
    build:
      context: .
    ports:
      - 5173:5173
    volumes:
      - .:/app
      - /app/node_modules
```

We need to edit the Dockerfile file with the following:

```Dockerfile
FROM node:20-alpine

RUN addgroup app && adduser -S -G app app

USER app

WORKDIR /app

COPY package*.json ./

USER root

RUN chown -R app:app .

USER app

RUN npm install

COPY . .

EXPOSE 5173

CMD npm run dev
```

**FROM** --> node with alpine linux

**RUN, USER** --> create user permissions for run command into the linux

**WORKDIR** --> creating /app directory and working this directory in the linux

**COPY** --> copying settings into the WORKDIR

**RUN npm install** --> create node environment, download packages, cerate folders

**COPY** --> copy this files into the WORKDIR directory (/app directory in the linux)

**EXPOSE** --> open a port for communicate

**CMD npm run dev** --> run the project files in the linux

create a .dockerignore file:

```
...
...
node_modules/
...
...
```

this will ignore the node_modules folder when copying to the image because the json files will be needed to install npm to avoid problems

we need to modify package.json file with the following: **(--host)**

```json
"scripts": {
    "dev": "vite --host",
    ...
    ...
}
```

We need run composer with the following:

```shell
docker compose up
```

and eventually we can see the page on the http://loclahost:5173 address

# Building and running your application

When you're ready, start your application by running:
`docker compose up --build`.

Your application will be available at http://localhost:5173.

# Deploying your application to the cloud

First, build your image, e.g.: `docker build -t myapp .`.
If your cloud uses a different CPU architecture than your development
machine (e.g., you are on a Mac M1 and your cloud provider is amd64),
you'll want to build the image for that platform, e.g.:
`docker build --platform=linux/amd64 -t myapp .`.

Then, push it to your registry, e.g. `docker push myregistry.com/myapp`.

Consult Docker's [getting started](https://docs.docker.com/go/get-started-sharing/)
docs for more detail on building and pushing.

# References

- [Docker's Node.js guide](https://docs.docker.com/language/nodejs/)
