# Install Docker Inside WSL2 Ubuntu (Without Docker Desktop)

This guide explains how to install Docker Engine directly inside WSL2 Ubuntu without using Docker Desktop.

---

# 1. Verify WSL2

Open PowerShell as Administrator:

```powershell
wsl --install
wsl -l -v
```

Ubuntu must show:

```text
VERSION 2
```

---

# 2. Open Ubuntu WSL

```bash
wsl
```

---

# 3. Update Packages

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 4. Install Required Packages

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

---

# 5. Add Docker GPG Key

```bash
sudo mkdir -p /etc/apt/keyrings
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

---

# 6. Add Docker Repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

# 7. Install Docker Engine

```bash
sudo apt update
```

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

---

# 8. Enable systemd in WSL

Edit configuration file:

```bash
sudo nano /etc/wsl.conf
```

Add below lines:

```ini
[boot]
systemd=true
```

Save and exit.

---

# 9. Restart WSL

From PowerShell:

```powershell
wsl --shutdown
```

Open Ubuntu again.

---

# 10. Start Docker

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

# 11. Verify Docker Installation

```bash
docker --version
```

```bash
docker run hello-world
```

Expected output:

```text
Hello from Docker!
```

---

# Optional: Run Docker Without sudo

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

# Check Docker Installation Location

```bash
which docker
```

```bash
docker info | grep "Docker Root Dir"
```

Usually Docker data is stored in:

```text
/var/lib/docker
```

---

# PostgreSQL Docker Example

Run PostgreSQL 17 container:

```bash
docker run -d \
--name pg17 \
-e POSTGRES_PASSWORD=postgres \
-p 5432:5432 \
postgres:17
```

Connect using psql:

```bash
psql -h localhost -U postgres
```

---

# Useful Docker Commands

## List running containers

```bash
docker ps
```

## List all containers

```bash
docker ps -a
```

## List images

```bash
docker images
```

## Stop container

```bash
docker stop <container_id>
```

## Remove container

```bash
docker rm <container_id>
```

---

# Benefits of Docker Inside WSL2

- No Docker Desktop required
- Lightweight setup
- Lower RAM usage
- Native Linux experience
- Good for PostgreSQL, Oracle, Patroni, pgBackRest labs
- Ideal for developers and DBAs

---

# Official References

- Docker Engine:
  https://docs.docker.com/engine/install/ubuntu/

- Microsoft WSL:
  https://learn.microsoft.com/windows/wsl/

---
