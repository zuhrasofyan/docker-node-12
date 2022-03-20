## Docker cheat sheet

Based on tutorial Docker + Node.js/express tutorial: Building dev/prod workflow with docker and Node.js: 
https://www.youtube.com/watch?v=gm_L69NHuHM

This version is complete and to run the docker, go to `DEV/PROD workflow` part instead.

### Without docker-compose

- build an image `docker build . -t zs/test-node12`
    - `-t` flag is to name the image
- Run a container from image: `docker run -v $(pwd):/app -v /app/node_modules --env-file ./.env -p 3000:4000 -d --name node-12 zs/test-node12`
    - `-v $(pwd):/app` flag is to bind volume in local machine current dir `$(pwd)` to container `/app` folder
    - `:ro` is read only to prevent container to write/add/modify files to our local machine
    - `-v /app/node_modules` prevent a specific folder in container (in this case, `/app/node_modules`) to be attached to node modules in local machine
    - `—env-file ./.env ` use of env-files from .env in local machine
    - `-p` 3000:4000 bind port <exposed:listening-to> port
    - `-d` detached mode means the terminal can continue to write other command (run container in background)
    - `—name node-12 zs/test-node12` name `container:local name`
    
### With docker-compose

### With docker-compose

To prevent using too long command to run a container as example above, and if we want to run multiple containers, then use 
docker-compose instead.
- running docker-compose: docker-compose up -d —build
    - `—build` to force rebuilding the image (in case we make new configuration to our docker image)
- stop docker-compose: `docker-compose down -v`

### DEV/PROD workflow

- Using different `docker-compose.<dev/prod>.yml` config to enhance base `docker-compose.yml`.
- then using `docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d —build`
- Stop: `docker-compose -f docker-compose.yml -f docker-compose.dev.yml down -v`


### General docker Command

- Delete container: `docker rm node-12 -f`
    - `-f` flag is to force delete even a running container
- Access volume in container using bash: `docker exec -it node-12 bash`
    - `-it` flag is shorthand for interactive and tty
- See the list of currently running containers: `docker ps`
- Clear build cache: `docker builder prune`
-  See the list of docker images: `docker image ls`
- Delete an image: `docker image rm <imageid>`
