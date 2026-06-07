MERN Docker Compose — Learning Notes
What is this project?
Building a 3-tier containerized application using the MERN stack:

M — MongoDB (Database layer)
E — Express (Backend framework)
R — React (Frontend/Presentation layer)
N — Node.js (Server runtime)


Why Docker Compose?
Instead of running multiple docker build and docker run commands manually for each container, Docker Compose lets you define and run all containers using a single YAML file.

Why do we need a Network?
When multiple containers are running, they need a common network to communicate with each other. Without a shared network, containers are isolated and can't talk to each other.

Docker comes with a default bridge network
We create a custom bridge network for better isolation and control
Same concept exists in Kubernetes


Steps to Containerize Manually (without Compose)

Create a custom Docker network
Write Dockerfile for frontend and backend
Build and run MongoDB with a volume mount (so data persists)
Build and run backend
Build and run frontend
Test by opening frontend and saving data to verify DB connection


Important: Start MongoDB before backend — backend tries to connect to DB as soon as it starts


Docker Compose File Structure
yamlservices:
  frontend:
    { frontend instructions }
  backend:
    { backend instructions }
    depends_on:
      - mongo
  mongo:
    { mongo instructions }

networks:
  demo:
    driver: bridge

volumes:
  mongo-data:
    driver: local

Key Concepts Learned
EXPOSE in Dockerfile

EXPOSE is just documentation — it does not change what port the app actually runs on
Always check docker logs to see what port the app is actually listening on
Port in -p flag must match the actual app port, not just the EXPOSE value

Container name as hostname

Inside Docker network, containers talk to each other using the service/container name, not localhost
Example: MongoDB connection string should be mongodb://mongo:27017 not mongodb://localhost:27017
The name used must exactly match the service name in docker-compose.yml

Connection String

Format: mongodb://host:port
In Docker: host = container/service name
In production: mongodb://username:password@host:27017/dbname

depends_on

Controls startup order in Docker Compose
Ensures MongoDB starts before backend


Errors Faced and Fixed
1. Dockerfile cannot be empty

Cause: File not saved before running docker build
Fix: Ctrl+S to save, then build again

2. Failed to load page on localhost

Cause: Wrong port mapping — used 5100 but Vite runs on 5173
Fix: Always check docker logs to see actual port, match -p flag to that

3. Invalid reference format on docker run

Cause: Used ~ in volume path which Windows doesn't understand
Fix: Use full path C:\Users\arjun\opt\db:/data/db

4. Port already allocated

Cause: Old container still holding the port
Fix: docker rm -f <container-name> then run again

5. Pull access denied for ongo

Cause: Typo in docker-compose.yml — ongo:8.0 instead of mongo:8.0
Fix: Corrected image name to mongo:8.0

6. DB showing blank after saving data

Cause: Backend connection string used localhost instead of service name
Fix: Changed to mongodb://mongo:27017 — must match exact service name in compose file

7. src refspec main does not match

Cause: Local branch was named compose not main
Fix: Pushed with git push -u origin compose

Docker Compose Steps to Run
docker compose down         # remove all containers
docker compose up --build   # build and start all containers
