# Docker commands and results 

Here we will document all the docker commands and results of those commands for future reference and learning. 

## Docker build 

Here we will build the image of a container using docker file

```bash
rinchhen-thing@Yat0slptp:~/Documents/new_devops_projects/docker/Docker_workshop/getting-started-app$ ls
Dockerfile  package.json  package-lock.json  README.md  spec  src
rinchhen-thing@Yat0slptp:~/Documents/new_devops_projects/docker/Docker_workshop/getting-started-app$ docker build -t getting-started .
[+] Building 17.9s (10/10) FINISHED                                                                                     docker:default
 => [internal] load build definition from Dockerfile                                                                              0.0s
 => => transferring dockerfile: 150B                                                                                              0.0s
 => [internal] load metadata for docker.io/library/node:24-alpine                                                                 7.4s
 => [auth] library/node:pull token for registry-1.docker.io                                                                       0.0s
 => [internal] load .dockerignore                                                                                                 0.0s
 => => transferring context: 64B                                                                                                  0.0s
 => [1/4] FROM docker.io/library/node:24-alpine@sha256:2bdb65ed1dab192432bc31c95f94155ca5ad7fc1392fb7eb7526ab682fa5bf14           0.0s
 => => resolve docker.io/library/node:24-alpine@sha256:2bdb65ed1dab192432bc31c95f94155ca5ad7fc1392fb7eb7526ab682fa5bf14           0.0s
 => [internal] load build context                                                                                                 0.0s
 => => transferring context: 1.80MB                                                                                               0.0s
 => CACHED [2/4] WORKDIR /app                                                                                                     0.0s
 => [3/4] COPY . .                                                                                                                0.1s
 => [4/4] RUN npm install --omit=dev                                                                                              7.6s
 => exporting to image                                                                                                            2.8s 
 => => exporting layers                                                                                                           2.1s 
 => => exporting manifest sha256:9d8cd525653d7cee33e4fe50490d04acf9299cf6cd4e3eba90e9ff7b0adbdc33                                 0.0s 
 => => exporting config sha256:ad1ba769900d81c4817b27b72782a3b98bd591ea5aadbcc11f7f222c3a86f5e6                                   0.0s 
 => => exporting attestation manifest sha256:95571fcbe8ab980aa336b69045b60368ea9b6595c587c9e528816566a0a482ab                     0.0s 
 => => exporting manifest list sha256:4427fc0157e4708374504a9aa9df8c791745907db9628f9bdc0ef6bcd2bfbe8c                            0.0s 
 => => naming to docker.io/library/getting-started:latest                                                                         0.0s
 => => unpacking to docker.io/library/getting-started:latest                                                                      0.6s
rinchhen-thing@Yat0slptp:~/Documents/new_devops_projects/docker/Docker_workshop/getting-started-app$ docker images
                                                                                                                   i Info →   U  In Use
IMAGE                    ID             DISK USAGE   CONTENT SIZE   EXTRA
**getting-started:latest**   **4427fc0157e4**        306MB         78.2MB         
 

```



