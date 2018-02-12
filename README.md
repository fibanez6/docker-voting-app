# DOCKER - Deploying an app to a Swarm

Example Voting App

### IMPORTANT

This project was cloned from [dockersamples/example-voting-app](
https://github.com/dockersamples/example-voting-app)

### REQUIREMENTS
* [Docker](https://www.docker.com/products/overview) 
* [Docker Swarm](https://docs.docker.com/engine/swarm/)

If you are on Mac or Windows, [Docker Compose](https://docs.docker.com/compose) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://docs.docker.com/compose/install/). If you're using [Docker for Windows](https://docs.docker.com/docker-for-windows/) on Windows 10 pro or later, you must also [switch to Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

### Installation

Run in this directory:
```
  > docker-compose up
```
The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).

Alternately, if you want to run it on a [Docker Swarm](https://docs.docker.com/engine/swarm/), first make sure you have a swarm. If you don't, run:
```
  > docker swarm init
```
Once you have your swarm, in this directory run:
```
  > docker stack deploy --compose-file docker-stack.yml vote
```

### Architecture

![Architecture diagram](architecture.png)

* A Python webapp which lets you vote between two options
* A Redis queue which collects new votes
* A .NET worker which consumes votes and stores them in…
* A Postgres database backed by a Docker volume
* A Node.js webapp which shows the results of the voting in real time


### Note

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.

### References 
* [Tutorial labs](https://docs.docker.com/samples/)
* [3.0 Deploying an app to a Swarm](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md) by PoppyBagel