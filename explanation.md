# Explanation of Containerization Design

## 1. Choice of Base Image
- **Backend:** Used `node:18-alpine` for a lightweight runtime and good compatibility with MongoDB drivers.
- **Frontend:** Used `nginx:alpine` to serve the React build; minimal and production-ready.
- **Database:** Used the official `mongo:6` image to ensure security patches and stability.

## 2. Dockerfile Directives
- Multi-stage builds reduce image size.
- Used `WORKDIR` for clean environment setup.
- `COPY` and `RUN npm install` for dependency management.
- `EXPOSE` and `CMD` to define runtime behavior.

## 3. Docker Compose Networking
- All containers use a custom **bridge network (`app-net`)** for isolated communication.
- `yolo-client` communicates with the backend through `http://yolo-backend:5000/api`.
- Port mappings:
  - Client → 3000:80
  - Backend → 5000:5000
  - MongoDB → 27017:27017

## 4. Volumes and Persistence
- Defined `app-mongo-data` as a named volume.
- Ensures MongoDB data persists across container restarts.

## 5. Git Workflow
- Multiple commits documenting each stage (Dockerfile creation, compose setup, debugging, tagging).
- Tags used:  
  - `edwinkorir38/yolo-backend:v1.0.0`  
  - `edwinkorir38/yolo-client:v1.0.1`
- Images pushed to DockerHub for easy verification.

## 6. Running & Debugging
- Start containers:  
  ```bash
  docker-compose up --build
