# DOCKER - Deploying an app to a Swarm

Example Voting App

### IMPORTANT

This project was cloned from [dockersamples/example-voting-app](
https://github.com/dockersamples/example-voting-app)

### REQUIREMENTS
* [Docker](https://www.docker.com/products/overview) 
* [Docker Swarm](https://docs.docker.com/engine/swarm/)

If you are on Mac or Windows, [Docker Compose](https://docs.docker.com/compose) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://docs.docker.com/compose/install/). If you're using [Docker for Windows](https://docs.docker.com/docker-for-windows/) on Windows 10 pro or later, you must also [switch to Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

### Architecture

![Architecture diagram](architecture.png)

* A Python webapp which lets you vote between two options
* A Redis queue which collects new votes
* A .NET worker which consumes votes and stores them in…
* A Postgres database backed by a Docker volume
* A Node.js webapp which shows the results of the voting in real time


### Note

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.


### DOCKER COMPOSE Installation

Build the images and start up:
```
  > docker-compose up -d
```
Check that the apps are running with docker-compose ps:
```
  > docker-compose ps
  
          Name                        Command               State                      Ports
----------------------------------------------------------------------------------------------------------------
db                         docker-entrypoint.sh postgres    Up      5432/tcp
dockervotingapp_result_1   nodemon server.js                Up      0.0.0.0:5858->5858/tcp, 0.0.0.0:5001->80/tcp
dockervotingapp_vote_1     python app.py                    Up      0.0.0.0:5000->80/tcp
dockervotingapp_worker_1   /bin/sh -c dotnet src/Work ...   Up
redis                      docker-entrypoint.sh redis ...   Up      0.0.0.0:32775->6379/tcp
```


The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).


Bring the app down:
```
  > docker-compose down --volumes
```

### DOCKER SWARM Installation

Alternately, if you want to run it on a [Docker Swarm](https://docs.docker.com/engine/swarm/), first make sure you have a swarm. If you don't, run:
```
  > docker swarm init
```
Once you have your swarm, in this directory run:
```
  > docker stack deploy --compose-file docker-stack.yml vote
```

to verify your stack has deployed, use `docker stack services vote`
```
  > docker stack services vote
ID                  NAME                MODE                REPLICAS            IMAGE                                    PORTS
dvf11j4c2ez5        vote_visualizer     replicated          0/1                 fibanez/visualizer:stable                *:8080->8080/tcp
j5ddtwlx81lt        vote_redis          replicated          1/1                 redis:alpine                             *:30005->6379/tcp
o6gw8g1pzfnm        vote_result         replicated          1/1                 fibanez/examplevotingapp_result:latest   *:5001->80/tcp
q3ohw0oax146        vote_vote           replicated          2/2                 fibanez/examplevotingapp_vote:latest     *:5000->80/tcp
qepc1vkoawfc        vote_db             replicated          1/1                 postgres:9.4
uotn9g8mt3up        vote_worker         replicated          1/1                 fibanez/examplevotingapp_worker:latest
```

The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).

Remove the stack from the swarm.
```
docker stack rm vote
```

Bring the stack down with `docker stack rm`:
```
  >  docker stack rm vote
```
Bring the registry down with `docker service rm`:
```
  >  docker service rm vote
```
To bring your Docker Engine out of swarm mode, use `docker swarm leave`
```
  > docker swarm leave --force
     
  Node left the swarm.
```

### References 
* [Tutorial labs](https://docs.docker.com/samples/)
* [3.0 Deploying an app to a Swarm](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md) -> [Docker labs](https://github.com/docker/labs)
* [deploy the stack to the swarm](https://docs.docker.com/engine/swarm/stack-deploy/#deploy-the-stack-to-the-swarm) -> from [Docker docs](https://docs.docker.com/)