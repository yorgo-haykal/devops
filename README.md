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

## Question 1-6
docker compose allows us to easily run and manage multiple containers at the same time, which makes it easier to manage full-stack apps on docker.

## Question 1-7
- docker compose up: starts all services defined in the compose.yml file.
- docker compose down: stops and removes running services.
- docker compose logs: prints logs to monitor the output of the running containers and debug issues.
- docker compose ps: list all the services along with their current status.

## Question 1-8
This docker-compose.yml defines a three-tier application with a backend, database, and HTTP server:
	•	Backend (Spring Boot) — runs the API that handles requests from the HTTP server and communicates with the database.
	•	Database (PostgreSQL) — stores persistent data; initialized automatically with SQL scripts from the /initdb directory.
	•	HTTPD (Apache) — acts as a simple web server or reverse proxy to serve the frontend and route API calls to the backend.

All services are connected via a shared bridge network (app-network), enabling them to communicate by service names (e.g., the backend connects to database:5432).
A named volume (db-data) is used to persist PostgreSQL data, ensuring that information is not lost when containers stop.

Environment variables are loaded securely from an external .env file instead of hardcoding them directly in the YAML, which improves maintainability and security.
