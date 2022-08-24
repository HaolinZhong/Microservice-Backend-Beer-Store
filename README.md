# Microservice-Backend-Beer-Store
This repo integrated various Spring Boot microservices &amp; Spring Cloud components repos, which constitue a backend service for a beer store.

Docker images of every microservices & components were built. 

- See [`How to deploy`](#how-to-deploy) to deploy the application **within 3 minutes** using docker. 

- See [`Ports & APIs`](#ports--apis) for testing the functionality of the application.

- See [`Project Architecture`](#project-architecture) for a summarization of microservices & Spring Cloud components.

- See `Bussiness Logic` for how microservices interact with each other.


## How to deploy

- **Before you run it**: Make sure you have installed docker and docker-compose. Allocate enough RAM for your docker containers (8GB may be enough), as 12 containers will be running simultaneously once started.

- In your terminal: 
  
  - `git clone [--recursive] https://github.com/HaolinZhong/Microservice-Backend-Beer-Store.git`:  `--recursive` is optional. Ignore it if you don't want source code of submodules in this repo.
  
  - `cd local\ deployment/`: change directory to the `local deployment` folder.

  - `docker compose up`: let docker download images and run them as containers according to `compose.yml`.

  - Wait for 1 or 2 minutes for all the service to be up and functioning.
  
- Terminate the application: `docker compose down`

## Ports & APIs

- `http://localhost:8761/`: Eureka Dashboard showing registered instances.
  
  - username: eurekaUser, password: eurekaPassword


## Project Architecture

![architechture](architecture.png)
