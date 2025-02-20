## Docker

### Pull an imaeg

```bash
docker pull <image>:<tag>
```

- For example:

```bash
docker docker pull postgres:17-alpine
```

### See images

```bash
(base) tieuma@Tieus-MacBook-Pro pcbook % docker images
REPOSITORY                       TAG         IMAGE ID       CREATED        SIZE
postgres                         17-alpine   80d3d7d6bb3d   6 days ago     385MB
gcr.io/k8s-minikube/kicbase      v0.0.45     7c93b02056db   5 months ago   1.67GB
gcr.io/k8s-minikube/kicbase      <none>      81df28859520   5 months ago   1.67GB
kodekloud/simple-prompt-docker   latest      8da0602f5bea   5 years ago    7.18MB
kodekloud/simple-webapp          latest      e5355b4c7804   6 years ago    131MB
kodekloud/webapp                 latest      f4ad0ca6b27f   7 years ago    661MB
```

### Start a container
```bash
docker run --image <container_name> -e <environment_variable> -d <image>:<tag>
```

- For example:

```bash
docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

### Port mapping

```bash
docker run --name <container_name> -e <environment_variable> -p <host_port:container_port> -d <image>:<tag>
```

- For example:

```bash
(base) tieuma@Tieus-MacBook-Pro pcbook %  docker run --name postgres17 -p 5432:5432 -e POSTGRES_USER=root -e POSTGRES_PASSWORD=secret -d postgres:17-alpine
fa72642fc47d1910f046c87bf8c221b841580b7e258332fcffa08cf87bd98247
```

### Run command in container 

```bash
docker exec -it <container_name_or_id> <command> [args]
```

- For example:

```bash
(base) tieuma@Tieus-MacBook-Pro pcbook % docker exec -it postgres17 psql -U root   
```

### View container logs

```bash
docker logs <container_name_or_id>
```