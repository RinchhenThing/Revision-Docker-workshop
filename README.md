# Docker Commands and Results

Here we will document all Docker commands and their outputs for future reference and learning.

## Docker Build

This command builds a Docker image from the Dockerfile in the current directory.

### Command

```bash
docker build -t getting-started .
```

### Output

```text
[+] Building 17.9s (10/10) FINISHED
...
=> => naming to docker.io/library/getting-started:latest
=> => unpacking to docker.io/library/getting-started:latest
```

### Verify the Image

```bash
docker images
```

Output:

```text
IMAGE                    ID             DISK USAGE   CONTENT SIZE   EXTRA
getting-started:latest   4427fc0157e4   306MB        78.2MB
```

**Image Name:** `getting-started:latest`

**Image ID:** `4427fc0157e4`


## Start an app container

This command is used to spin up an container using the previously built image

### Command
```bash
docker run -d -p 127.0.0.1:3000:3000 getting-started
```

### Output

```text
$ docker run -d -p 127.0.0.1:3000:3000 getting-started
afbe6eb8d0f50dc4f98d9eab810a6d933745e12870cdd2ce640ddc89cd7d46b5

```

### Verify the container

```bash
docker ps

```

Output: 

```text
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                      NAMES
afbe6eb8d0f5   getting-started   "docker-entrypoint.s…"   39 seconds ago   Up 38 seconds   127.0.0.1:3000->3000/tcp   intelligent_liskov

```

**Container Name:** `getting-started`
**Container ID:** `afbe6eb8d0f5`


