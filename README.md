#  Docker_ci
# 🚀 FastAPI + Docker Full Guide (Beginner to Pro)

This is a complete step-by-step guide to build, run, and manage a FastAPI project using Docker, including naming images, containers, background mode (-d), and GitHub Actions CI.

---

# 📁 1. Project Structure

```
fastapi-docker-app/
│
├── app.py
├── requirements.txt
├── Dockerfile
│
└── .github/
    └── workflows/
        └── ci.yml
```

---

# 🐍 2. app.py (FastAPI)

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello from devops-training!"}
    
    

```

---

# 📦 3. requirements.txt

```
fastapi
uvicorn
```

---

# 🐳 4. Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

# ⚙️ 5. GitHub Actions (CI)

```yaml
name: Build FastAPI Docker

on:
  push:
    branches: ['main']

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v4
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v4

      - name: Build and push
        uses: docker/build-push-action@v7
        with:
          push: true
          tags: abdelrahmanamin2000/app:latest
```

# 🚀 6. Docker Commands (IMPORTANT)

---

# 🟢 Development Mode (Volume - LIVE CODE)

Use volume when you want changes in Python code to update instantly without rebuilding:

```bash
docker run -d \
  -p 8000:8000 \
  -v $(pwd):/app \
  --name fastapi-api-container \
  fastapi-api:v1
```

🧠 Meaning:

- `-v $(pwd):/app` → sync your local code with container
- Any change in `.py` files updates instantly
- NO need to rebuild image

---

# 🔵 Production Mode (No Volume)

```bash
docker run -d \
  -p 8000:8000 \
  --name fastapi-api-container \
  fastapi-api:v1
```

🧠 Meaning:

- Stable environment
- No live code sync
- Used for servers (VPS / cloud)

---

# 🧠 Rule

> Use Volume for development, Docker image for production.

---

# 🚀 6. Docker Commands (IMPORTANT)

---

## 🟢 Build Image (with naming)

```bash
docker build -t fastapi-api:v1 .
```

---

## 🟢 Run Container (background + name)

```bash
docker run -d \
  --name fastapi-api-container \
  -p 8000:8000 \
  fastapi-api:v1
```

---

# 🧠 Explanation

| Part            | Meaning           |
| --------------- | ----------------- |
| -d              | run in background |
| --name          | container name    |
| -p              | port mapping      |
| fastapi-api\:v1 | image name        |

---

# 🌐 7. Test API

```
http://localhost:8000/hello
```

```
http://localhost:8000/add?a=5&b=10
```

---

# 📊 8. Container Management

## Show running containers

```bash
docker ps
```

## Show all containers

```bash
docker ps -a
```

## Stop container

```bash
docker stop fastapi-api-container
```

## Start container again

```bash
docker start fastapi-api-container
```

## Remove container

```bash
docker rm fastapi-api-container
```

## View logs

```bash
docker logs fastapi-api-container
```

## Remove image

```bash
docker rmi fastapi-api:v1
```

---

# 🔥 9. Full Workflow

```
1. Write code (app.py)
2. Build image
3. Run container (-d)
4. Test API
5. Push to GitHub
6. GitHub Actions builds automatically
```

---

# 🧠 Key Concepts

- Image = blueprint
- Container = running app
- Docker = isolated environment
- GitHub Actions = automation

---

# 🚀 Golden Rule

> You build an image, then you run a container.

---

# 👨‍💻 Next Step (Advanced)

You can upgrade this project with:

- Docker Compose (API + DB)
- PostgreSQL integration
- Redis cache
- Full CI/CD deploy to VPS
- AI model inside FastAPI

---

# 🎯 Done!

---

# 🐳 Docker Hub Deployment (IMPORTANT)

To push your image to Docker Hub, follow these steps:

## 1. Login to Docker Hub

```bash
docker login
```

---

## 2. Tag your image

```bash
docker tag fastapi-api:v1 abdelrahmanamin2000/fastapi-api:v1
```

---

## 3. Push image to Docker Hub

```bash
docker push abdelrahmanamin2000/fastapi-api:v1
```

---

## 🧠 Why Docker Hub?

- Share your app with others
- Deploy on VPS easily
- No need to rebuild on every machine

---

# ⚡ Reduce Image Size (VERY IMPORTANT)

Instead of using full Python image:

```dockerfile
FROM python:3.10
```

Use slim version:

```dockerfile
FROM python:3.10-slim
```

🧠 Benefits:

- Smaller image size (GB → MB)
- Faster build
- Faster deployment

---

# 🧹 .dockerignore (VERY IMPORTANT)

Create a file named `.dockerignore` to reduce image size:

```text
__pycache__/
*.pyc
*.pyc
*.pyo
*.pyd
.git/
venv/
.env
node_modules/
dist/
build/
.cache/
.idea/
.vscode/
*.pt
*.pth
*.ckpt
```

🧠 Why?

- Prevents unnecessary files from entering Docker image
- Reduces size significantly
- Improves security and speed

---

# 🔐 GitHub Secrets & Docker Hub Setup (IMPORTANT)

To make GitHub Actions work with Docker Hub safely, you must configure **Secrets and Variables**.

---

## 🟢 1. Docker Hub Username (GitHub Variable)

Go to GitHub repository:

```
Settings → Secrets and variables → Actions → Variables
```

Add:

```
Name: DOCKERHUB_USERNAME
Value: your_dockerhub_username
```

🧠 Example:
```
abdelrahmanamin2000
```

---

## 🟡 2. Docker Hub Access Token (GitHub Secret)

Go to Docker Hub:

```
https://hub.docker.com/
```

Then:

```
Account Settings → Security → New Access Token
```

- Create token (name: github-ci)
- Copy token (IMPORTANT: shown only once)   -> read & write

---

Now go to GitHub:

```
Settings → Secrets and variables → Actions → Secrets
```

Add:

```
Name: DOCKERHUB_TOKEN
Value: <your_dockerhub_access_token>
```

---

## 🧠 Why this is needed?

```yaml
username: ${{ vars.DOCKERHUB_USERNAME }}
password: ${{ secrets.DOCKERHUB_TOKEN }}
```

- `vars` → safe non-sensitive config (username)
- `secrets` → encrypted sensitive data (token)

---

## 🔥 Security Rule

> Never put Docker Hub password or token in code or GitHub repo.

---

# 🚀 Final Flow (Docker Hub + Deploy)

```text
1. docker build -t fastapi-api:v1 .
2. docker run -d  -p 8000:8000  --name fastapi-api-container  fastapi-api:v1
3. docker login
4. docker tag fastapi-api:v1 username/fastapi-api:v1
5. docker push username/fastapi-api:v1
6. deploy anywhere using docker pull
```

---

# 🎯 You now have a full beginner → production Docker workflow.

You now have a full beginner → production Docker workflow.

