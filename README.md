# 🐳 Django → Docker → EC2 Deploy

A simple Django web app containerized with Docker and deployed on AWS EC2. Built as a hands-on practice project following the full deploy flow: **local dev → Docker → GitHub → EC2**.

---

## 🗂️ Project Structure

```
myproject/
├── myapp/
│   ├── views.py          # Renders the main page
│   └── urls.py           # Route: / → home view
├── myproject/
│   ├── settings.py       # Django config (no database)
│   ├── urls.py           # Includes myapp urls
│   └── wsgi.py           # Entry point for Gunicorn
├── templates/
│   └── myapp/
│       └── index.html    # Django → Docker → EC2 deploy guide page
├── manage.py
├── requirements.txt      # Django + Gunicorn
├── Dockerfile
└── .gitignore
```

---

## ⚙️ Tech Stack

| Layer      | Technology              |
|------------|-------------------------|
| Backend    | Django (Python)         |
| Server     | Gunicorn                |
| Container  | Docker                  |
| Hosting    | AWS EC2 (Ubuntu 22.04)  |
| Version Control | GitHub             |

> No database used — this is a static-view Django app.

---

## 🚀 Getting Started

### 1. Run Locally (without Docker)

```bash
pip install -r requirements.txt
python manage.py runserver
# Visit: http://127.0.0.1:8000
```

### 2. Run with Docker

```bash
docker build -t myapp .
docker run -p 8000:8000 myapp
# Visit: http://localhost:8000
```

---

## ☁️ Deploying on AWS EC2

### Step 1 — Launch EC2 Instance
- OS: **Ubuntu 22.04 LTS**
- Instance type: `t2.micro` (free tier)
- Security Group: Allow **port 22** (SSH) and **port 8000** (app)
- Download your `.pem` key file

### Step 2 — SSH into the Instance

```bash
chmod 400 your-key.pem
ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
```

### Step 3 — Install Docker & Git on EC2

```bash
sudo apt update && sudo apt install -y docker.io git
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
newgrp docker
```

### Step 4 — Clone & Run

```bash
git clone https://github.com/loprekira063/Docker_Django_app.git
cd Docker_Django_app
docker build -t myapp .
docker run -d -p 8000:8000 myapp
```

### Step 5 — Visit Your Live App

```
http://<your-ec2-public-ip>:8000
```

---

## 📌 Notes

- SSH key auth is used for GitHub (HTTPS + PAT had permission issues on macOS Keychain)
- No static file collection needed — templates are served directly by Django in dev mode
- For production, consider adding Nginx as a reverse proxy in front of Gunicorn

---

## 📄 License

MIT
