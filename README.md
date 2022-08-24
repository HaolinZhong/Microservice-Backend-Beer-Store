# Microservice-Backend-Beer-Store
This repo integrated various Spring Boot microservices &amp; Spring Cloud components repos, which constitue a backend service for a beer store.

Docker images of every microservices & components were built. 

- See [`How to deploy`](#how-to-deploy) to deploy the application **within 3 minutes** using docker. 
- See [`Project Architecture`](#project-architecture) for a summarization of microservices & Spring Cloud components.
- See [`Ports & APIs`](#ports--apis) for testing the functionality of the application.
- See `Bussiness Logic` for how microservices interact with each other.


## How to deploy

- **Before you run it**: Make sure you have installed docker and docker-compose. Allocate enough RAM for your docker containers (8GB may be enough), as 12 containers will be running simultaneously once started.

- In your terminal: 
  
  - `git clone [--recursive] https://github.com/HaolinZhong/Microservice-Backend-Beer-Store.git`:  `--recursive` is optional. Ignore it if you don't want source code of submodules in this repo.
  
  - `cd local\ deployment/`: change directory to the `local deployment` folder.

  - `docker compose up`: let docker download images and run them as containers according to `compose.yml`.

  - Wait for 1 or 2 minutes for all the service to be up and functioning.
  
- Terminate the application: `docker compose down`



## Project Architecture

![architechture](architecture.png)



## Ports & APIs

- `http://localhost:8761/`: Eureka Dashboard showing registered instances.

  - username: eurekaUser, password: eurekaPassword

    

- `http://localhost: 9411/zipkin`: Zipkin Dashboard for visulization of traces.

  - try any of the gateway api below to let `Gateway` appear on the tracing map.
  - try to manually shut down `Inventory Service` to let `Inventory Failover` appear on the tracing map.

  

- `http://localhost:9090/`: Spring Cloud Gateway

  - requests redirect to brewery service controller:
    - `api/v1/beer`: 
      - `GET`: return all beers basic information, paged. add `?showInventory=true` to show beer stock.
      - `POST`: save the beer object post if it is valid.

    - `api/v1/beer/{beerId}`:
      - `GET` :  return infomation of the given beer designated by id. BeerId example: `095c83ee-c1bf-4933-8389-21bb6cedcd3f`. add `?showInventory=true` to show beer stock.
      - `PUT`: update the given beer if beer object post is valid.

    - `api/v1/beerUpc/{upc}`:
      - `GET`: return infomation of the given beer designated by UPC.

  - requests redirect to inventory service controller:
    - authentication required. username: `MyInventory`, `MyInventoryPw`
    - `api/v1/beer/{beerId}/inventory`
      - `GET` : return all inventory of the designated beer.

  - requests redirect to order service controller:
    - `/api/v1/customers`
      - `GET`: get all customers basic information (no sensitive info). paged.

    - `/api/v1/customers/{customerId}/orders`
      - `GET`: get all orders created by the designated customer
      - `POST`: create a new order for the designated customer

    - `/api/v1/customers/{customerId}/orders/{orderId}`
      - `GET`: get the designated order information for the customer

    - `/api/v1/customers/{customerId}/orders/{orderId}/pickup`
      - `PUT`: change the designated order status to `PICKED_UP`, if the current order status is `ALLOCATED`.

