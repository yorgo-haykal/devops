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

## Question 1-9
To make my Docker images usable on other machines, I published them to Docker Hub.
I logged in, tagged the images with my Docker Hub namespace and version, and pushed them:
`docker login`
`docker tag my-database yorgohaykal/my-database:1.0`
`docker push yorgohaykal/my-database:1.0`

## Questions 1-10
To make the image available to every system that needs to deploy or update it, reliably and automatically.

# TP part 02 - Github Actions
## Question 2-1
Testcontainers allow integration tests to run against real Dockerized dependencies instead of mocks or local installs.

## Question 2-2
We use secured varibales to protect sensitive information like passwords and tokens, that way we don't publish them and they don't appear in logs.

## Question 2-3
We put needs: test-backend so that it only runs when test-backend is completed, if test-backend fails it doesn't run which makes sense because we don't want to push a faulty image.

## Question 2-4
We push our Docker images so that other machines can pull the exact same version of our application and deploy it reliably.

# TP part 03 - Ansible
## Question 3-1
The inventory file tells Ansible which server to manage and how to connect to it (SSH user, key, hostname).
Example commands:
- ansible all -i inventories/setup.yml -m ping → check connectivity
- ansible all -i inventories/setup.yml -m setup → get system info (facts)
- ansible all -i inventories/setup.yml -m apt -a "name=apache2 state=absent" --become → enforce package state

## Question 3-2
This playbook installs and configures Docker automatically on the remote server.
It updates the APT cache, adds Docker’s official repository and GPG key, installs Docker CE along with Python tools required by Ansible, and ensures the Docker service is running.
This provides a clean and repeatable Docker setup with no manual actions needed.

## Question 3-3
This playbook deploys the entire application stack to the production server. It installs Docker, creates the network and volume, and starts the three services (DB, backend, proxy).
Secrets are handled via Ansible Vault. Only the proxy is exposed to the public.

## Question 3-4
Automatically deploying every image pushed to the registry is not safe, because untested or compromised images could reach production. To secure it, we should enforce CI validations, control which branches trigger deployment, require approvals, and deploy only signed and versioned images.
