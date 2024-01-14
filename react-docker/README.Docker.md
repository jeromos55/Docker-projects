## 2. React-docker project

create a react project

```
npm create vite@latest react-docker
```

select React --> TypeScript after go into the folder

```
cd react-docker
```

create a Dockerfile:

```
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
node_modules/
```

this will ignore the node_modules folder when copying to the image because the json files will be needed to install npm to avoid problems

we need to modify package.json file with the following: **(--host)**

```
"scripts": {
    "dev": "vite --host",
    ...
    ...
}
```

we can build this image with the following:

```
docker build -t react-docker .
```

and run the image with the following:

```
docker run -p 5173:5173 react-docker
```

-p 5173:5173 --> we need to link to each other the container's port and the computer's port

this create a container and runs the '**npm run dev**' command in the /app directory on the linux
eventually we can see that page on the http://localhost:5173/ address

we can modify the code and synchronize the own folders and files to the container files with the following:

```
docker run -p 5173:5173 -v "${pwd}:/app" -v /app/node_modules react-docker
```

${pwd} --> the local directory

-v "${pwd}:/app" --> we need to link to each other local directory and the container's /app directory

-v /app/node_modules --> we need to link to each other the container's /app/node_modules directory and the local directory

if we running the image and we opening up that the localhost address and modifying the local directory files the container will synchronize immediately the files.

we can deploying the image to the cloud with the following:

```
docker login
docker tag react-docker user_name/react-docker
docker push user_name/react-docker
```

eventually others can see and use this image

# Building and running your application

When you're ready, start your application by running:
`docker compose up --build`.

Your application will be available at http://localhost:3000.

# Deploying your application to the cloud

First, build your image, e.g.: `docker build -t myapp .`.
If your cloud uses a different CPU architecture than your development
machine (e.g., you are on a Mac M1 and your cloud provider is amd64),
you'll want to build the image for that platform, e.g.:
`docker build --platform=linux/amd64 -t myapp .`.

Then, push it to your registry, e.g. `docker push myregistry.com/myapp`.

Consult Docker's [getting started](https://docs.docker.com/go/get-started-sharing/)
docs for more detail on building and pushing.

### References

- [Docker's Node.js guide](https://docs.docker.com/language/nodejs/)
