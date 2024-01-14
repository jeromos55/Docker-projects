# Docker-projects

Archive and create docker projects

I create this repository because I find this youtube video https://www.youtube.com/watch?v=GFgJkfScVNU and I want to notes that and documentation for if we will need to create same as docker image and container later it will be help our projects. This video teach me a new way to use docker.

1. Hello-docker project
2. React-docker project
3. Vite-docker project
4. mern-docker project
5. next-docker project

## 1. Hello-docker project

```
mkdir hello-docker
cd hello-docker
```

cerate a Dockerfile:

```
FROM  node:20-alpine
WORKDIR /app
COPY . .
CMD node hello.js
```

**FROM** --> alpine linux

**WORKDIR** --> creating /app directory and working this directory in the linux

**COPY** --> copy this directory files into the WORKDIR (/app directory in the linux)

**CMD** --> run this command on the WORKDIR directory

Than run **build** command to build an image with name of hello-docker:

```
docker build -t hello-docker .
```

-t **_(target)_** image_name

. **_(copy all files and directory)_**

we can run the image and go into the image:

```
docker run -it hello-docker sh
```

and we run the node:

```
/app# node hello.js
Hello Docker!
```

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

**RUN npm install** --> create node environment download packages and cerate folders

**COPY** --> copy this directory files into the WORKDIR (/app directory in the linux)

**EXPOSE** --> open a port for communicate

**CMD npm run dev** --> run the project files in the linux

than create a .dockerignore file:

```
node_modules/
```

this will ignore the node_modules folder to copying to the image because firstly we need to the \*.json to setting npm installing files in the image for avoid problems

than we need to modify package.json file with the following: **(--host)**

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

-p 5173:5173 --> we need to connect to the container's port and the computer's port

this create a container and runs the '**npm run dev**' command in the /app directory on the linux
eventually we can see that page on the http://localhost:5173/ address

we can modify the code and synchronize the own folders and files to the container files with the following:

```
docker run -p 5173:5173 -v "${pwd}:/app" -v /app/node_modules react-docker
```

${pwd} --> the local directory

-v "${pwd}:/app" --> we need to connect to local directory and the container's /app directory

-v /app/node_modules --> we need to connect to the container's /app/node_modules directory and the local directory

if we running the image and seeing that the localhost address and modifying the local directory files the container will synchronize immediately the files.

we can publish the image to the DOCKERHUB with the following:

```
docker login
docker tag react-docker user_name/react-docker
docker push user_name/react-docker
```

eventually others can see and use this image

# continue comming soon
