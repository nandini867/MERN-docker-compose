# A simple MERN stack application

MongoDB — Database
Express — Backend framework
React + Vite — Frontend
Node.js — Server runtime


Running Manually (without Compose)
1. Create a network
docker network create demo
2. Build and run frontend
cd mern/frontend
docker build -t mern-frontend .
docker run --name=frontend --network=demo -d -p 5173:5173 mern-frontend
3. Run MongoDB
docker run --network=demo --name mongo -d -p 27017:27017 -v C:\Users\<your-username>\opt\db:/data/db mongo:latest

#Note: On Windows use full path instead of ~/opt/data

4. Build and run backend
cd mern/backend
docker build -t mern-backend .
docker run --name=backend --network=demo -d -p 5050:5050 mern-backend
Open http://localhost:5173 to verify

Running with Docker Compose (recommended)
docker compose up --build
Open http://localhost:5173
To stop:
docker compose down
