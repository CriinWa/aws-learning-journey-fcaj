---
title: "Deploy Docker on EC2"
date: 2026-08-11
weight: 8
chapter: false
pre: " <b> 5.3.6. </b> "
---
The system is deployed as a production demo on AWS, specifically an **AWS EC2** instance.

## Architecture overview

![1786475288795](image/_index.vi/1786475288795.png)

Services are packaged and managed with Docker Compose as two independent containers.

| Container | Port | Role |
| ------------------- | ---- | --------------------------------------- |
| **API** | 8000 | Backend<br />FastAPI + QAService |
| **Streamlit** | 8501 | User interface |

Main config files:

- `deploy/Dockerfile`: packages the app into a Docker image.
- `deploy/docker-compose.yml`: orchestrates the containers.
- `deploy/entrypoint.sh`: startup script based on `APP_MODE` (api / streamlit).

## Create an Amazon EC2 instance in AWS Management Console

Sign in to AWS Management Console.
In the search bar, type EC2 and open the EC2 service.
![1786463354216](image/_index.vi/1786463354216.png)

In the upper-right corner, choose the nearest Region.
![1786463420321](image/_index.vi/1786463420321.png)

On the EC2 Dashboard, choose Launch instance.

![1786463557808](image/_index.vi/1786463557808.png)

Under Name and tags, enter a name for the instance.

![1786463808143](image/_index.vi/1786463808143.png)

Under Application and OS Images (Amazon Machine Image), choose the OS.

![1786464044683](image/_index.vi/1786464044683.png)

Under Instance type, choose a suitable size.
![1786464101261](image/_index.vi/1786464101261.png)

Under Key pair, choose Create new key pair.

![1786464218855](image/_index.vi/1786464218855.png)

Name the key.
Choose Key pair type: `RSA`
Choose Private key file format:

* Choose `.pem` for OpenSSH in Terminal (Linux/Mac/Windows 10+) or a recent PuTTY.
* Choose `.ppk` for older PuTTY.
* Choose **Create key pair** and save the file locally (AWS lets you download it only once).

![1786464254052](image/_index.vi/1786464254052.png)

Configure Network Settings
![1786464691491](image/_index.vi/1786464691491.png)

Configure Storage
By default AWS creates one EBS volume (8 GiB for Linux, 30 GiB for Windows)
![1786464706662](image/_index.vi/1786464706662.png)

Choose Launch instance
![1786464732104](image/_index.vi/1786464732104.png)

## Deploy the application on Amazon EC2

After the instance is running, connect over SSH with the `.pem` key.

On Windows, open **Command Prompt** (CMD) and move to the folder that contains the `.pem` file.

```bash
ssh -i law-chatbot-key.pem ubuntu@18.143.187.153
```

After login, update the system and install deployment tools.

```bash
sudo apt update
sudo apt upgrade -y
```

Install Docker

```bash
sudo apt install -y docker.io
```

Start Docker and enable it on boot

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Then copy the project source onto the instance.

```bash
git clone https://github.com/KhanhKoy/vietnamese-legal-llmops.git
cd vietnamese-legal-llmops
git fetch origin
git checkout master
git pull origin master

# Create .env from the sample
cp .env.sample .env

# Edit .env
nano .env
```

Important `.env` values for Compose

```
API_URL=http://api:8000/ask
DATABASE_URL=...
GEMINI_API_KEY=...
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
AWS_REGION=...
```

Use the real project values, then save (in nano: Ctrl+O → Enter → Ctrl+X).

Build the Docker image from source

```bash
docker compose -f deploy/docker-compose.yml build
```

Docker reads the Dockerfile, installs dependencies, and creates images for the services.

After a successful build, start the services in the background:

```bash
docker compose -f deploy/docker-compose.yml up -d
```

Check container status

```bash
docker ps
```

If containers show Up or healthy, the system started successfully.

## Post-startup checks

| Check | Command / URL |
| ------------ | ---------------------------------------------------------------------------------- |
| API | `curl http://localhost:8000/` or Swagger `http://<EC2_PUBLIC_IP>:8000/docs` |
| Streamlit UI | `http://<ec2-public-ip>:8501` |
