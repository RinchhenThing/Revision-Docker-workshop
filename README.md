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


## Rebuild Image After Making Changes

After modifying the application source code, rebuild the Docker image to include the latest changes.

### Modify Application Code

```bash
nano src/static/js/app.js
```

### Rebuild the Image

```bash
docker build -t getting-started .
```

### Verify the New Image

```bash
docker images
```

Output:

```text
IMAGE                    ID             DISK USAGE   CONTENT SIZE
getting-started:latest   30d973b7bf9e   306MB        78.2MB
```

**New Image ID:** `30d973b7bf9e`

---

## Replace the Running Container

Attempting to start a new container on port 3000 resulted in an error because another container was already using the port.

### Attempt to Run the New Container

```bash
docker run -dp 127.0.0.1:3000:3000 getting-started
```

Output:

```text
docker: Error response from daemon:
Bind for 127.0.0.1:3000 failed: port is already allocated
```

### Check Running Containers

```bash
docker ps
```

Output:

```text
CONTAINER ID   IMAGE          PORTS                      NAMES
afbe6eb8d0f5   4427fc0157e4   127.0.0.1:3000->3000/tcp   intelligent_liskov
```

### Stop the Existing Container

```bash
docker stop afbe6eb8d0f5
```

### Remove the Existing Container

```bash
docker rm afbe6eb8d0f5
```

### Confirm No Containers Are Running

```bash
docker ps
```

Output:

```text
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS   PORTS   NAMES
```

### Start a Container Using the Updated Image

```bash
docker run -dp 127.0.0.1:3000:3000 getting-started
```

### Verify the New Container

```bash
docker ps
```

Output:

```text
CONTAINER ID   IMAGE             PORTS                      NAMES
5ad36b7d6dbb   getting-started   127.0.0.1:3000->3000/tcp   unruffled_khorana
```

**Container ID:** `5ad36b7d6dbb`

This confirms that the old container was replaced with a new container created from the updated Docker image.

