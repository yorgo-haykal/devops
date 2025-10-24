# TP part 01
## Question 1-1
It's better to use the -e flag because we don't want to put sensitive information in the Dockerfile which might be publicly accessible, and we do't want to hardcode our env variables so that it can be used by more than one user for with multiple instances of env vars.

## Question 1-2
We need a volume to be attached to our postgres container because if there's no volume attached, the data inside of the database is wiped evertime the container is destroyed, but databases usually need to persist.

## Question 1-3
Pull the image
`docker build -t yorgohaykal/database:latest .`

Create a Docker network:
`docker network create app-network`

Run PostgreSQL:
`docker run -d -p 5432:5432 --network app-network -v pgdata:/var/lib/postgresql/data --name database -e POSTGRES_DB=db -e POSTGRES_USER=use -e POSTGRES_PASSWORD=pwd yorgohaykal/database`

Start Adminer
`docker run -d --name adminer --network app-network -p 8080:8080 adminer`

## Question 1-4
We need a multistage build to separate the container images used to build the project from the one used to run it, that way we can drastically reduce the run image size thus making it quicker. The explaination of each step of the dockerfile is in its comments.

## Question 1-5
A reverse proxy is used to protect backend servers, balance traffic, centralize access, improve performance, and ensure reliability by acting as a single secure entry point between clients and internal services.
