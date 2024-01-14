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

We can run the image and go into the image:

```
docker run -it hello-docker sh
```

and we run the node:

```
/app# node hello.js
Hello Docker!
```
