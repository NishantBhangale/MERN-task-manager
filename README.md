# MERN Task Manager

A MERN application for basic tasks management with Docker support.

![image](https://user-images.githubusercontent.com/86913048/227101123-f8a35258-9c21-4479-86e8-055659ab75e2.png)

---

## Table of Contents

- [Features](#features)
- [Tools and Technologies](#tools-and-technologies)
- [Dependencies](#dependencies)
- [Dev-dependencies](#dev-dependencies)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Docker Setup](#docker-setup)
- [Docker Architecture](#docker-architecture)
- [Backend API](#backend-api)
- [Frontend Pages](#frontend-pages)
- [npm Scripts](#npm-scripts)
- [Useful Links](#useful-links)
- [Contact](#contact)

---

## Features

### User-side features

- Signup
- Login
- Logout
- Add tasks
- View tasks
- Update tasks
- Delete tasks

### Developer-side features

- Toasts for success and error messages
- Form validations in frontend and backend
- Fully Responsive Navbar
- Token based Authentication
- Use of 404 page for wrong urls
- Relevant redirects
- Global user state using Redux
- Custom Loaders
- Use of layout component for pages
- Use of theme colors
- No external CSS files needed (made using Tailwind CSS)
- Usage of Tooltips
- Dynamic document titles
- Redirect to previous page after login
- Use of various React hooks
- Custom hook also used (`useFetch`)
- Routes protection
- Middleware for verifying the user in backend
- Use of different HTTP status codes for sending responses
- Standard practices followed

---

## Tools and Technologies

- HTML
- CSS
- JavaScript
- Tailwind CSS
- Node.js
- Express.js
- React
- Redux
- MongoDB
- Docker
- Docker Compose
- Nginx

---

## Dependencies

Following are the major dependencies of the project:

- axios
- react
- react-dom
- react-redux
- react-router-dom
- react-toastify
- redux
- redux-thunk
- bcrypt
- cors
- dotenv
- express
- jsonwebtoken
- mongoose

---

## Dev-dependencies

Following are the major dev-dependencies of the project:

- nodemon
- concurrently

---

## Prerequisites

### Without Docker

- Node.js must be installed on the system.
- You should have a MongoDB database.
- You should have a code editor (preferred: VS Code).

### With Docker

- Docker must be installed.
- Docker Compose must be available.

---

## Installation and Setup

### 1. Install all dependencies

```sh
npm run install-all
```

### 2. Configure environment variables

Create a `.env` file inside the `backend` folder.

You can use the `.env.example` file as a reference.

Do not commit the actual `.env` file to GitHub.

### 3. Start the application

```sh
npm run dev
```

### 4. Open the application

```text
http://localhost:3000
```

---

# Docker Setup

The project also supports running the complete MERN application using Docker and Docker Compose.

The application is divided into separate containers:

- MongoDB
- Backend
- Frontend

The frontend is served using **Nginx** instead of running the Node.js development server.

This helps keep the frontend container lightweight and suitable for serving the production build.

---

## Docker Files

The project contains Docker configuration for both frontend and backend:

```text
MERN-task-manager/
│
├── backend/
│   ├── Dockerfile
│   ├── .env
│   └── .env.example
│
├── frontend/
│   └── Dockerfile
│
├── docker-compose.yml
└── README.md
```

---

## Docker Compose Services

### MongoDB

MongoDB runs using the official MongoDB image.

```text
mongo:7
```

MongoDB data is stored in a Docker named volume so that the data remains available even if the MongoDB container is recreated.

```text
tm-mongo-data
```

A healthcheck is also configured for MongoDB so the backend waits for MongoDB to become healthy before starting.

---

### Backend

The backend is built using its own Dockerfile.

```text
backend/Dockerfile
```

The backend runs on:

```text
5000
```

The host port mapping is:

```text
5000:5000
```

The backend receives environment variables from:

```text
backend/.env
```

The Docker Compose configuration uses:

```yaml
env_file:
  - ./backend/.env
```

---

### Frontend

The frontend is built using its own Dockerfile.

```text
frontend/Dockerfile
```

The frontend production build is served using **Nginx**.

The container exposes port:

```text
80
```

The host port is:

```text
8081
```

Therefore, the application can be accessed at:

```text
http://localhost:8081
```

Port mapping:

```text
8081:80
```

Using Nginx to serve the frontend production build also reduces the frontend image size compared with running the application using the Node.js development server.

---

## Docker Architecture

```text
                    ┌──────────────────────┐
                    │      Browser         │
                    │                      │
                    │ http://localhost:8081│
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      Frontend        │
                    │       Nginx          │
                    │      Port 80         │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       Backend        │
                    │     Node + Express   │
                    │      Port 5000       │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       MongoDB         │
                    │       Port 27017      │
                    │                      │
                    │   Persistent Volume  │
                    └──────────────────────┘
```

All containers communicate through the Docker network:

```text
to-do-net
```

---

## Build Docker Images

From the project root:

```sh
docker compose build
```

To build without using the cache:

```sh
docker compose build --no-cache
```

---

## Start Containers

Start the complete application:

```sh
docker compose up
```

Run in detached mode:

```sh
docker compose up -d
```

---

## Check Running Containers

```sh
docker ps
```

---

## Check Docker Images

```sh
docker images
```

Example:

```text
mern-task-manager-backend:latest
mern-task-manager-frontend:latest
```

The frontend image is kept smaller by using Nginx to serve the production build. (95mb.)

---

## View Container Logs

### All services

```sh
docker compose logs
```

### Backend

```sh
docker compose logs backend
```

### Frontend

```sh
docker compose logs frontend
```

### MongoDB

```sh
docker compose logs mongodb
```

Follow logs continuously:

```sh
docker compose logs -f
```

---

## Stop Containers

```sh
docker compose down
```

To stop containers and remove the containers:

```sh
docker compose down
```

The MongoDB named volume is kept unless it is explicitly removed.

---

## Remove Containers and Volumes

To remove containers and the MongoDB volume:

```sh
docker compose down -v
```

> Warning: Removing the volume will delete the MongoDB data stored in that Docker volume.

---

## Environment Variables

An example environment file is provided:

```text
backend/.env.example
```

Create the actual environment file:

```text
backend/.env
```

Use `.env.example` as a reference and add your own values.

The actual `.env` file should not be committed to GitHub.

---

## Access the Application

When running with Docker Compose:

### Frontend

```text
http://localhost:8081
```

### Backend

```text
http://localhost:5000
```

### MongoDB

```text
localhost:27017
```

---

## Backend API

```text
POST     /api/auth/signup

POST     /api/auth/login

GET      /api/tasks

GET      /api/tasks/:taskId

POST     /api/tasks

PUT      /api/tasks/:taskId

DELETE   /api/tasks/:taskId

GET      /api/profile
```

---

## Frontend Pages

```text
/                 Home Screen (Public home page for guests and private dashboard
                  (tasks) for logged-in users)

/signup           Signup page

/login            Login page

/tasks/add        Add new task

/tasks/:taskId    Edit a task
```

---

## npm Scripts

### At root

```text
npm run dev
```

Starts both backend and frontend.

```text
npm run dev-server
```

Starts only backend.

```text
npm run dev-client
```

Starts only frontend.

```text
npm run install-all
```

Installs all dependencies and dev-dependencies required at root, at frontend and at backend.

---

### Inside frontend folder

```text
npm start
```

Starts frontend in development mode.

```text
npm run build
```

Builds the frontend for production.

```text
npm test
```

Launches the test runner in the interactive watch mode.

```text
npm run eject
```

This will remove the single build dependency from the frontend.

---

### Inside backend folder

```text
npm run dev
```

Starts backend using nodemon.

```text
npm start
```

Starts backend without nodemon.

---

## Useful Links

### This Project

- GitHub Repo: https://github.com/aayush301/MERN-task-manager

### Official Docs

- Reactjs docs: https://reactjs.org/docs/getting-started.html
- npmjs docs: https://docs.npmjs.com/
- MongoDB docs: https://docs.mongodb.com/manual/introduction/
- GitHub docs: https://docs.github.com/en/get-started/quickstart/hello-world

### YouTube Tutorials

- Expressjs: https://youtu.be/L72fhGm1tfE
- React: https://youtu.be/EHTWMpD6S_0
- Redux: https://youtu.be/1oU_YGhT7ck

### Download Links

- Node.js: https://nodejs.org/
- VS Code: https://code.visualstudio.com/

### Cheatsheets

- Git cheatsheet: https://education.github.com/git-cheat-sheet-education.pdf
- VS Code keyboard shortcuts: https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf
- CSS Selectors Cheatsheet: https://frontend30.com/css-selectors-cheatsheet/

---

## Contact

- Email: aayush5521186@gmail.com
- Linkedin: https://www.linkedin.com/in/aayush12/