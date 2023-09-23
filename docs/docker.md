# Docker

## Show running containers
```
docker ps
```

## Show all containers including stopped ones
```
docker ps -a
```

## Show container log
```
docker logs <container-id>
```

## Show container log and follow the log in realtime
```
docker logs <container-id> --follow
```

## Access into container
```
docker exec -it <container-id> /bin/sh
```
or (depending on container type)
```
docker exec -it <container-id> /bin/bash
```

## Remove a container
```
docker rm <container-id>
```

## Remove all stopped containers
```
docker rm $(docker ps -a -q)
```

## List Docker images
```
docker images
```

## Remove a Docker image
```
docker rmi <id>
```

## Remove all idle images
```
docker image prune -a
```

# Tips to use Docker Compose for localhost development
You should not tie your project codebase to Docker Compose for localhost development. That's fast and easy way, but a bad practices. You PC will be overwhelmed by a lot of duplicated containers and that will ruin your SSD storage space. Do this instead.
Create a file named `~/dev/docker-compose.yml` (or put it whenever you want).
Put whatever services you want to use for your engineer's life, e.g: PostgreSQL, MySQL, Redis, etc.
```yaml
version: '3.1'

services:
  postgres:
    image: postgres
    restart: always
    ports:
      - "5432:5432"
  redis:
    image: redis
    restart: always
    ports:
      - "6379:6379"
```
Now, start your containers on terminal `cd ~/Apps/docker_dev_containers && docker-compose up`.
Use Ctrl+C to stop the running whenever you want.
