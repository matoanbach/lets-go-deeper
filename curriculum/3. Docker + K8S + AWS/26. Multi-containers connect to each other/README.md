## How to user docker network to connect 2 stand-alone containers

```bash
docker run --image simplebank -p 8080:8080 simplebank:1.0
```

### Make them connect to the same network in Docker


```bash
(base) tieuma@Tieus-MacBook-Pro simple_bank % docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks
```

```bash
docker network create bank-network
```

```bash
docker network connect simplebank-network simplebank
docker network connect simplebank-network postgres17
```

### Inspect docker network

```bash
docker container inspect postgres17
```

### Start simplebank again

```bash
docker run -d --name simplebank --network bank-network -p 8080:8080 -e DB_SOURCE="postgresql://root:secret@postgres17:5432/simple_bank?sslmode=disable" simplebank:1.0
```