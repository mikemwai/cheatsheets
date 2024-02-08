# Docker

## Basic Commands

## 1) Build with Docker
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

