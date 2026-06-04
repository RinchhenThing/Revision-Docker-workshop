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



## Push Docker Image to Docker Hub

To push an image to Docker Hub, the image must first be tagged with your Docker Hub username and repository name.

### Attempt to Push the Local Image

```bash
docker push rinchhenthing/getting-started:latest
```

Output:

```text
The push refers to repository [docker.io/rinchhenthing/getting-started]
tag does not exist: rinchhenthing/getting-started:latest
```

The error occurred because no local image existed with the tag `rinchhenthing/getting-started:latest`.

### Verify Existing Images

```bash
docker images
```

Output:

```text
IMAGE                    ID             DISK USAGE   CONTENT SIZE
getting-started:latest   30d973b7bf9e   306MB        78.2MB
```

### Tag the Image for Docker Hub

```bash
docker tag getting-started:latest rinchhenthing/getting-started:latest
```

This creates a new tag that points to the same image.

### Verify the New Tag

```bash
docker images
```

Output:

```text
IMAGE                                  ID             DISK USAGE   CONTENT SIZE
getting-started:latest                 30d973b7bf9e   306MB        78.2MB
rinchhenthing/getting-started:latest   30d973b7bf9e   306MB        78.2MB
```

Notice that both image names share the same image ID (`30d973b7bf9e`), meaning they reference the same image.

### Push the Tagged Image to Docker Hub

```bash
docker push rinchhenthing/getting-started:latest
```

Output:

```text
The push refers to repository [docker.io/rinchhenthing/getting-started]
09552ba7698b: Pushed
6a0ac1617861: Pushed
eea2c8c31119: Pushed
c7fd89fde416: Pushed
414f813165cf: Pushed
c2f5dc65cfdb: Pushed
f6ca80bbdbeb: Pushed
8b6a932c48ed: Pushed
latest: digest: sha256:30d973b7bf9e210eb0ad225a1db8cfed6376a83bd3b34b93e722035426ac8c6c size: 856
```

### Result

The image was successfully uploaded to Docker Hub.

**Repository:** `rinchhenthing/getting-started`

**Tag:** `latest`

**Image ID:** `30d973b7bf9e`

**Digest:** `sha256:30d973b7bf9e210eb0ad225a1db8cfed6376a83bd3b34b93e722035426ac8c6c`



## Docker Volumes

Docker volumes provide persistent storage for containers. Data stored in a volume remains available even if the container is removed and recreated.

### Demonstrating Container Isolation

Create a file inside a temporary Alpine container:

```bash
docker run --rm alpine touch greeting.txt
```

Docker automatically downloaded the Alpine image because it was not available locally.

Output:

```text
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
...
Status: Downloaded newer image for alpine:latest
```

Start another temporary Alpine container and check for the file:

```bash
docker run --rm alpine stat greeting.txt
```

Output:

```text
stat: can't stat 'greeting.txt': No such file or directory
```

This demonstrates that files created inside one container are not automatically available in another container.

---

## Create a Docker Volume

Create a volume named `todo-db`:

```bash
docker volume create todo-db
```

Output:

```text
todo-db
```

The volume will be used to persist the application's data.

---

## Remove the Existing Container

Check the running container:

```bash
docker ps
```

Output:

```text
CONTAINER ID   IMAGE             PORTS                      NAMES
5ad36b7d6dbb   getting-started   127.0.0.1:3000->3000/tcp   unruffled_khorana
```

Remove the container:

```bash
docker rm -f 5ad36b7d6dbb
```

Output:

```text
5ad36b7d6dbb
```

---

## Start a Container with a Mounted Volume

Run the application and mount the volume:

```bash
docker run -dp 127.0.0.1:3000:3000 \
  --mount type=volume,src=todo-db,target=/etc/todos \
  getting-started
```

Output:

```text
8de0621bc8911b1d4a81b73dd4f7604d8f2f56908499dcfae0db5b8d5ea47f6f
```

Verify the container is running:

```bash
docker ps
```

Output:

```text
CONTAINER ID   IMAGE             PORTS                      NAMES
8de0621bc891   getting-started   127.0.0.1:3000->3000/tcp   vigilant_yonath
```

---

## Verify Data Persistence

Remove the container:

```bash
docker rm -f 8de0621bc891
```

Output:

```text
8de0621bc891
```

Start a new container using the same volume:

```bash
docker run -dp 127.0.0.1:3000:3000 \
  --mount type=volume,src=todo-db,target=/etc/todos \
  getting-started
```

Output:

```text
cdc9bc3697512a577957edec442a810a2523c318e87bdd9ae9cd9603a75fbbef
```

Because the same volume was mounted, any application data stored in `/etc/todos` remains available to the new container.

---

## Inspect the Volume

View detailed information about the volume:

```bash
docker volume inspect todo-db
```

Output:

```json
[
  {
    "CreatedAt": "2026-06-04T09:20:28+05:45",
    "Driver": "local",
    "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
    "Name": "todo-db",
    "Scope": "local"
  }
]
```

### Important Information

* **Volume Name:** `todo-db`
* **Driver:** `local`
* **Mountpoint:** `/var/lib/docker/volumes/todo-db/_data`
* **Purpose:** Persist application data independently of the container lifecycle.

This confirms that the application's data is stored in a Docker-managed volume rather than inside the container filesystem.
