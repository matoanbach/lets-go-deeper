## How to build a small Golang Docker image with a multistage Dockerfile

Application code -> buid image to Docker -> Ship it to AWS

### Set up the Dockerfile

```bash
FROM golang:1.23
WORKDIR /app
COPY . .
RUN go build -o main main.go

EXPOSE 8080
CMD ["/app/main"]
```

- Note: image size of simple bank is too large

```bash
(base) tieuma@Tieus-MacBook-Pro simple_bank % docker images
REPOSITORY                       TAG         IMAGE ID       CREATED          SIZE
simplebank                       1.0         28ed61f375f0   38 seconds ago   1.67GB
postgres                         17-alpine   80d3d7d6bb3d   11 days ago      385MB
gcr.io/k8s-minikube/kicbase      v0.0.45     7c93b02056db   5 months ago     1.67GB
gcr.io/k8s-minikube/kicbase      <none>      81df28859520   5 months ago     1.67GB
kodekloud/simple-prompt-docker   latest      8da0602f5bea   5 years ago      7.18MB
kodekloud/simple-webapp          latest      e5355b4c7804   6 years ago      131MB
kodekloud/webapp                 latest      f4ad0ca6b27f   7 years ago      661MB
```

### Build the docker

```bash
(base) tieuma@Tieus-MacBook-Pro simple_bank % docker build --help
Start a build

Usage:  docker buildx build [OPTIONS] PATH | URL | -

Start a build

Aliases:
  docker build, docker builder build, docker image build, docker buildx b

Options:
      --add-host strings              Add a custom host-to-IP mapping (format: "host:ip")
      --allow strings                 Allow extra privileged entitlement (e.g.,
                                      "network.host", "security.insecure")
      --annotation stringArray        Add annotation to the image
      --attest stringArray            Attestation parameters (format:
                                      "type=sbom,generator=image")
      --build-arg stringArray         Set build-time variables
      --build-context stringArray     Additional build contexts (e.g., name=path)
      --builder string                Override the configured builder instance (default
                                      "desktop-linux")
      --cache-from stringArray        External cache sources (e.g., "user/app:cache",
                                      "type=local,src=path/to/dir")
      --cache-to stringArray          Cache export destinations (e.g., "user/app:cache",
                                      "type=local,dest=path/to/dir")
      --call string                   Set method for evaluating build ("check", "outline",
                                      "targets") (default "build")
      --cgroup-parent string          Set the parent cgroup for the "RUN" instructions
                                      during build
      --check                         Shorthand for "--call=check" (default )
  -D, --debug                         Enable debug logging
  -f, --file string                   Name of the Dockerfile (default: "PATH/Dockerfile")
      --iidfile string                Write the image ID to a file
      --label stringArray             Set metadata for an image
      --load                          Shorthand for "--output=type=docker"
      --metadata-file string          Write build result metadata to a file
      --network string                Set the networking mode for the "RUN" instructions
                                      during build (default "default")
      --no-cache                      Do not use cache when building the image
      --no-cache-filter stringArray   Do not cache specified stages
  -o, --output stringArray            Output destination (format: "type=local,dest=path")
      --platform stringArray          Set target platform for build
      --progress string               Set type of progress output ("auto", "plain", "tty",
                                      "rawjson"). Use plain to show container output
                                      (default "auto")
      --provenance string             Shorthand for "--attest=type=provenance"
      --pull                          Always attempt to pull all referenced images
      --push                          Shorthand for "--output=type=registry"
  -q, --quiet                         Suppress the build output and print image ID on success
      --sbom string                   Shorthand for "--attest=type=sbom"
      --secret stringArray            Secret to expose to the build (format:
                                      "id=mysecret[,src=/local/secret]")
      --shm-size bytes                Shared memory size for build containers
      --ssh stringArray               SSH agent socket or keys to expose to the build
                                      (format: "default|<id>[=<socket>|<key>[,<key>]]")
  -t, --tag stringArray               Name and optionally a tag (format: "name:tag")
      --target string                 Set the target build stage to build
      --ulimit ulimit                 Ulimit options (default [])
```

### Reduce the size of the image

```bash
# Build stage
FROM golang:1.23-alpine AS builder
WORKDIR /app
COPY . .
RUN go build -o main main.go

# Run stage
FROM alpine
WORKDIR /app
COPY --from=builder /app/main .

EXPOSE 8080
CMD ["/app/main"]
```

- Note: image size of simplebank has been reduced significantly

```bash
(base) tieuma@Tieus-MacBook-Pro simple_bank % docker images
REPOSITORY                       TAG         IMAGE ID       CREATED          SIZE
simplebank                       1.0         28ed61f375f0   38 seconds ago   1.67GB
postgres                         17-alpine   80d3d7d6bb3d   11 days ago      385MB
gcr.io/k8s-minikube/kicbase      v0.0.45     7c93b02056db   5 months ago     1.67GB
gcr.io/k8s-minikube/kicbase      <none>      81df28859520   5 months ago     1.67GB
kodekloud/simple-prompt-docker   latest      8da0602f5bea   5 years ago      7.18MB
kodekloud/simple-webapp          latest      e5355b4c7804   6 years ago      131MB
kodekloud/webapp                 latest      f4ad0ca6b27f   7 years ago      661MB
```


