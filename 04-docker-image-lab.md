
## 🧪 **Docker Lab Exercise: Build, Tag & Push 5 Images**

### 🔧 **Lab Objectives**

* Create Dockerfiles using different base images
* Use key Dockerfile instructions (`FROM`, `ENV`, `RUN`, `COPY`, `EXPOSE`, `WORKDIR`, `VOLUME`, `CMD`, `ENTRYPOINT`)
* Build and tag images
* Push to Docker Hub
* Create containers with port and volume mappings

---

## 📁 **Setup**

```bash
mkdir docker-lab
cd docker-lab
```

---

## 🧩 **Dockerfile 1 – Ubuntu with Apache**

**File:** `Dockerfile.ubuntu-apache`

```Dockerfile
FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt update && apt install -y apache2
WORKDIR /var/www/html
COPY index.html .
EXPOSE 80
VOLUME ["/var/www/html"]
CMD ["apache2ctl", "-D", "FOREGROUND"]
```

**index.html**

```html
<html><body><h1>Ubuntu Apache Server</h1></body></html>
```

**Build & Run**

```bash
docker build -f Dockerfile.ubuntu-apache -t ubuntu-apache:v1 .
docker tag ubuntu-apache:v1 your_dockerhub_username/ubuntu-apache:v1
docker login
docker push your_dockerhub_username/ubuntu-apache:v1
docker run -d -p 8080:80 --name ubuntu-apache ubuntu-apache:v1
```

---

## 🧩 **Dockerfile 2 – CentOS Stream 10 with httpd**

**File:** `Dockerfile.centos-httpd`

```Dockerfile
FROM quay.io/centos/centos:stream10
RUN dnf install -y httpd && dnf clean all
COPY index.html /var/www/html/
WORKDIR /var/www/html
EXPOSE 80
VOLUME ["/var/www/html"]
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

**index.html**

```html
<html><body><h1>CentOS Stream 10 HTTPD</h1></body></html>
```

**Build & Run**

```bash
docker build -f Dockerfile.centos-httpd -t centos-httpd:v1 .
docker tag centos-httpd:v1 your_dockerhub_username/centos-httpd:v1
docker push your_dockerhub_username/centos-httpd:v1
docker run -d -p 8081:80 --name centos-httpd centos-httpd:v1
```

---

## 🧩 **Dockerfile 3 – Nginx with custom homepage**

**File:** `Dockerfile.nginx`

```Dockerfile
FROM nginx:latest
WORKDIR /usr/share/nginx/html
COPY index.html .
EXPOSE 80
VOLUME ["/usr/share/nginx/html"]
CMD ["nginx", "-g", "daemon off;"]
```

**index.html**

```html
<html><body><h1>Custom NGINX Page</h1></body></html>
```

**Build & Run**

```bash
docker build -f Dockerfile.nginx -t custom-nginx:v1 .
docker tag custom-nginx:v1 your_dockerhub_username/custom-nginx:v1
docker push your_dockerhub_username/custom-nginx:v1
docker run -d -p 8082:80 --name custom-nginx custom-nginx:v1
```

---

## 🧩 **Dockerfile 4 – Python Flask App**

**File:** `Dockerfile.python-flask`

```Dockerfile
FROM python:3.10-slim
ENV APP_HOME=/app
WORKDIR $APP_HOME
COPY app.py .
RUN pip install flask
EXPOSE 5000
CMD ["python", "app.py"]
```

**app.py**

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Flask running in Docker!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

**Build & Run**

```bash
docker build -f Dockerfile.python-flask -t python-flask:v1 .
docker tag python-flask:v1 your_dockerhub_username/python-flask:v1
docker push your_dockerhub_username/python-flask:v1
docker run -d -p 5000:5000 --name flask-app python-flask:v1
```

---

## 🧩 **Dockerfile 5 – Alpine Custom Script**

**File:** `Dockerfile.alpine-script`

```Dockerfile
FROM alpine:latest
WORKDIR /usr/local/bin
COPY hello.sh .
RUN chmod +x hello.sh
ENTRYPOINT ["./hello.sh"]
```

**hello.sh**

```bash
#!/bin/sh
echo "Hello from Alpine custom container!"
```

**Build & Run**

```bash
docker build -f Dockerfile.alpine-script -t alpine-script:v1 .
docker tag alpine-script:v1 your_dockerhub_username/alpine-script:v1
docker push your_dockerhub_username/alpine-script:v1
docker run --name alpine-hello alpine-script:v1
```

---

## 🧹 **Cleanup Commands**

```bash
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
docker rmi $(docker images -q)
```

---

## ✅ **Validation Tasks**

1. List images and tags:

   ```bash
   docker images
   ```
2. List running containers:

   ```bash
   docker ps
   ```
3. Access web containers from browser:

   * Ubuntu Apache → [http://localhost:8080](http://localhost:8080)
   * CentOS HTTPD → [http://localhost:8081](http://localhost:8081)
   * Nginx → [http://localhost:8082](http://localhost:8082)
4. Verify Flask:

   ```bash
   curl http://localhost:5000
   ```
5. Check Alpine output:

   ```bash
   docker logs alpine-hello
   ```

---
