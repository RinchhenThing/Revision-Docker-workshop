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
