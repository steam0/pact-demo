# Installation and running the demo application

### 1. Install Java JDK

You will need Java installed in order to run the demo applications. You will need at least JDK 1.8 installed. You can
download the jdk from this location: https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html  

### 2. (Optional) Download an IDE

I recommend downloading and installing an IDE before this workshop. I prefer and recommend Intellij IDEA. You may 
download and start a 30-day trial from this location: https://www.jetbrains.com/idea/download/index.html


### 3. Install Docker

You will need docker installed locally in order to run some applications. I recommend downloading and installing Docker 
Desktop from this location: https://www.docker.com/products/docker-desktop


### 4. Download repository

Clone this repository from  Github and open it in your IDE and run it! This will download all the neccesary dependencies.


### 5. Download and run a postgres database using docker

Just run this command in your preferred shell. This will download and install a postgres database.

`docker run --name pact-db -p 6543:5432 -e POSTGRES_USER=pact -e POSTGRES_PASSWORD=password -d postgres`

### 6. Download and run the pactbroker using docker

Just run this command in your preferred shell. THis will download and install a pactbroker.

`docker run --name pactbroker --link pact-db:postgres -e PACT_BROKER_DATABASE_USERNAME=pact -e PACT_BROKER_DATABASE_PASSWORD=password -e PACT_BROKER_DATABASE_HOST=postgres -e PACT_BROKER_DATABASE_NAME=pactbroker -d -p 3000:80 dius/pact-broker`


### 7. Get excited for the workshop!

See you all on Tuesday



## Install and configure a pact broker database
1. Start postgres db

```
docker run --name pact-db -p 6543:5432 -e POSTGRES_USER=pact -e POSTGRES_PASSWORD=password -d postgres
```

2. Connect to db

```
docker run -it --link pact-db:postgres --rm postgres sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U pact'
```

3. Run script

```
CREATE USER pactuser WITH PASSWORD 'password';
CREATE DATABASE pactbroker WITH OWNER pactuser;
GRANT ALL PRIVILEGES ON DATABASE pactbroker TO pactuser;
```

## Run Pact Broker

```
docker run --name pactbroker --link pact-db:postgres -e PACT_BROKER_DATABASE_USERNAME=pactuser -e PACT_BROKER_DATABASE_PASSWORD=password -e PACT_BROKER_DATABASE_HOST=postgres -e PACT_BROKER_DATABASE_NAME=pactbroker -d -p 3000:80 dius/pact-broker
```
