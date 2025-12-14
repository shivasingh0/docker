# 🚀 From 0 to Production – Full-Stack App Deployment
```md
This project demonstrates how to take a full-stack application from **local development to production on a VPS** using **Docker, Docker Compose, and Nginx**.

---

## 📌 Tech Stack

- Frontend: React / Next.js
- Backend: Node.js + Express
- Database: MongoDB / PostgreSQL
- Containerization: Docker, Docker Compose
- Server: Ubuntu VPS
- Reverse Proxy: Nginx
- SSL: Certbot (Let’s Encrypt)

---

## 📂 Project Structure

```

.
├── frontend/
│   ├── Dockerfile
│   └── package.json
├── backend/
│   ├── Dockerfile
│   └── package.json
├── docker-compose.yml
├── .env
└── README.md

````

---

## ⚙️ Prerequisites

- VPS (Ubuntu 20.04+ recommended)
- Domain name (optional but recommended)
- Git installed
- Docker & Docker Compose
- SSH access to VPS

---

## 🧑‍💻 Local Setup

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
````

---

### 2️⃣ Environment Variables

Create a `.env` file:

```env
NODE_ENV=production
PORT=5000
DATABASE_URL=your_database_url
```

---

### 3️⃣ Run Locally (Without Docker)

**Backend**

```bash
cd backend
npm install
npm run dev
```

**Frontend**

```bash
cd frontend
npm install
npm run dev
```

---

## 🐳 Docker Setup

### 4️⃣ Backend Dockerfile

```dockerfile
FROM node:20-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .
EXPOSE 5000

CMD ["npm", "start"]
```

---

### 5️⃣ Frontend Dockerfile

```dockerfile
FROM node:20-alpine AS build

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
```

---

### 6️⃣ Docker Compose

```yaml
version: "3.9"

services:
  backend:
    build: ./backend
    container_name: backend
    ports:
      - "5000:5000"
    env_file:
      - .env
    restart: always

  frontend:
    build: ./frontend
    container_name: frontend
    ports:
      - "3000:80"
    restart: always
```

---

### 7️⃣ Run with Docker

```bash
docker compose up -d --build
```

---

## 🌍 VPS Deployment

### 8️⃣ Connect to VPS

```bash
ssh root@your_server_ip
```

---

### 9️⃣ Install Docker on VPS

```bash
sudo apt update
sudo apt install -y docker.io docker-compose
```

Enable Docker:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

---

### 🔟 Deploy Application

```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
docker compose up -d --build
```

Your app is now live on:

```
http://your_server_ip:3000
```

---

## 🌐 Nginx Reverse Proxy

### 1️⃣1️⃣ Install Nginx

```bash
sudo apt install nginx -y
```

---

### 1️⃣2️⃣ Nginx Configuration

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Enable config:

```bash
sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

## 🔒 SSL (HTTPS)

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com
```

---

## 🧪 Useful Commands

```bash
docker ps
docker logs backend
docker compose down
docker compose up -d
```

---

## 📈 Production Checklist

* ✅ Dockerized frontend & backend
* ✅ VPS deployment
* ✅ Nginx reverse proxy
* ✅ HTTPS enabled
* ✅ Environment variables secured
