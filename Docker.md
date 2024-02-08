# Docker

## Basic Commands

## 1) Build with Docker
- Before building a docker image, ensure you have created a Docker file with the format:
  
```sh
  # syntax=docker/dockerfile:1

  FROM node:18-alpine
  WORKDIR /app
  COPY . .
  RUN yarn install --production
  CMD ["node", "src/index.js"]
  EXPOSE 3000
```

`N/B`: Change the commands as per the programming language used for the project.

- Build a docker image:

```sh
  docker build --tag=projectname .
```

`N/B`: For the image name, it is best to use the project name to avoid confusion. In addition to that, it creates an image with the tag (name) `projectname`.

- Run the container hosting the server component:

```sh
  docker run --name=projectname --rm --detach projectname
```

`N/B`: For the container name, it is best to use the project name to avoid confusion. In addition to that, it also starts a container named `projectname`.

- Inside the `projectname` container created previously, invoke the client binary:

```sh
  docker exec -it projectname /bin/client
```

`N/B`: For the container name, it is best to use the project name to avoid confusion. Moreover, it also starts a container named `projectname`.

- Stop the container after testing:

```sh
  docker stop projectname
```

