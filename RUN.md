# RUN.md - MELI Project Lifting Guidelines

This project contains three main components: frontend, backend and their code coverage versions (frontend-coverage and
backend-coverage), all orchestrated using Docker Compose.

## 🔧 Prerequisites

- Docker [Click here to install](https://docs.docker.com/get-docker/)
- Docker Compose [Click here to install](https://docs.docker.com/compose/install/)
- Git [Click here to install](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## 🧱 Relevant structure for deployment

```
.
├── .env.example           # Base file with port configuration
├── docker-compose.yml     # Definition of services
├── .gitmodules            # Git frontend and backend submodules
├── backend/               # Backend submodule
│   └── .env.example
│   └── Dockerfile
│   └── DockerfileCoverage
├── frontend/              # Frontend submodule
│   └── .env.example
│   └── Dockerfile
│   └── DockerfileCoverage
```

## 🚀 Steps to implement the project.

### 1. Clone the repository
   ```bash
   git clone https://github.com/jucamo0713/tech-test-meli.git
   cd tech-test-meli
   ```
---
### 2. Initialize Git submodules

This project uses Git submodules for the `frontend` and `backend` folders. Run:

```bash
git submodule init
git submodule update
```

> If you cloned using `--recurse-submodules`, this step is not necessary.

---

### 3. Configure environment variables

#### a. Create the root `.env` file

Copy from the provided example:

```bash
cp .env.example .env
```

Make sure to define the following ports (you can customize them if needed):

```dotenv
FRONT_PORT=4000
BACK_PORT=4001
FRONT_COVERAGE_PORT=4002
BACK_COVERAGE_PORT=4003
```

#### b. Set up environment variables for each service

Inside both the `frontend/` and `backend/` directories, copy the example env files:

```bash
cp frontend/.env.example .env.frontend
cp backend/.env.example .env.backend
```

Edit each `.env` file as needed.

---

### 4. Build and run the containers

```bash
docker-compose up --build
```

This will start the following services:

* `frontend`
* `backend`
* `frontend-coverage`
* `backend-coverage`

---

### 🩺 Backend Healthcheck

The backend includes a healthcheck that runs every 60 seconds by calling:

```
http://localhost:3000/api/shared/health-check
```

Internally, it uses `wget` in the container:

```docker
wget --quiet --spider http://localhost:3000/api/shared/health-check || exit 1
```

---

## ✅ Access and Verification

Once everything is up:

* Frontend: `http://localhost:${FRONT_PORT}`
* Backend: `http://localhost:${BACK_PORT}/api`
* Frontend Coverage: `http://localhost:${FRONT_COVERAGE_PORT}`
* Backend Coverage: `http://localhost:${BACK_COVERAGE_PORT}`

if you used the default ports from the `.env` file.

* Frontend: http://localhost:4000
* Backend: http://localhost:4001
* Frontend Coverage: http://localhost:4002
* Backend Coverage: http://localhost:4002
---
### 📚 Backend Swagger Documentation

The backend service includes interactive API documentation powered by **Swagger**.

Once the backend is running, Swagger will be available at the following path:

```
http://localhost:${BACK_PORT}/${SWAGGER_PATH}
```

Where:

* `${BACK_PORT}` is set in your root `.env` file (e.g. `4001`)
* `${SWAGGER_PATH}` is defined in the `.env.backend` file, like:

```env
SWAGGER_PATH=/api-docs
```

So if you used the values above, the Swagger UI would be accessible at:

http://localhost:4001/api-docs

Make sure the `SWAGGER_PATH` is correctly set in the backend environment to access the documentation.

---
## 🛑 Shut Down

To stop all containers:

```bash
docker-compose down
```
