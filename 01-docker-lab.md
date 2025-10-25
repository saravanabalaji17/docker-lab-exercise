---

# 🧪 **Docker Lab Exercise**

---

## **🧰 Prerequisites**

* Linux system (Ubuntu 22.04/24.04 recommended)
* Docker installed and running

---

## **STEP 1: Verify Docker Setup**

### 🧩 Commands

```bash
docker version
docker --version
docker info
```

### 📘 Explanation

* `docker version` → Shows client and server versions.
* `docker info` → Gives system-wide info like storage driver, cgroup driver, number of containers, and images.

### ✅ Checkpoint

Ensure both **Client** and **Server (Engine)** versions are visible.
If `Cannot connect to the Docker daemon` → run:

```bash
sudo systemctl start docker
```

---

## **STEP 2: Explore and Pull Images**

### 🧩 Commands

```bash
docker search ubuntu
docker search nginx
```

### 📘 Explanation

Searches Docker Hub for available images.

### 🧩 Pull Images

```bash
docker pull ubuntu:20.04
docker pull ubuntu:22.04
docker pull centos:8
docker pull nginx
```

### ✅ Checkpoint

```bash
docker images
```

Should list all pulled images.

### 🔍 Verify Image Layers

```bash
docker history nginx
```

---

## **STEP 3: Running Containers (Interactive vs Detached)**

### 🧩 Run Interactively

```bash
docker run -it --name mynginx nginx
```

* `-it` → interactive + TTY
* `--name` → assign a readable name
* `nginx` → base image

Inside container:

```bash
hostname
exit
```

> 🔹 **Note:** Exiting interactive mode stops the container.

### 🧩 Run in Detached Mode

```bash
docker run -it -d --name httpd-container httpd
```

Check status:

```bash
docker ps
```

### ✅ Checkpoint

You should see your container running in background with a random port mapping.

---

## **STEP 4: Container Management**

### 🧩 List Containers

```bash
docker ps
docker ps -a
```

### 🧩 Access Running Container

```bash
docker exec -it httpd-container /bin/bash
date
exit
```

### 🧩 Stop/Start/Restart Containers

```bash
docker stop httpd-container
docker start httpd-container
docker restart httpd-container
```

### 🧩 Kill a Container (Force Stop)

```bash
docker kill httpd-container
```

---

## **STEP 5: Container Removal & Cleanup**

### 🧩 Remove Stopped Containers

```bash
docker rm httpd-container
```

### 🧩 Remove Running Container (Force)

```bash
docker rm -f mynginx
```

### 🧩 Remove Images

```bash
docker rmi ubuntu:20.04
docker rmi 54c9d81cbb44
```

### ✅ Checkpoint

```bash
docker images
docker ps -a
```

Ensure only desired containers/images remain.

---

## **STEP 6: Port Publishing (Host ↔ Container)**

### 🧩 Run Web Servers with Port Mapping

```bash
docker run -it -d --name httpd-web-container -p 9000:80 httpd
docker run -it -d --name nginx-web-container -p 8800:80 nginx:latest
```

### 📘 Explanation

* `-p 9000:80` → Maps host port 9000 to container port 80
* Useful for accessing containers via browser.

### ✅ Verification

```bash
docker ps
```

Output example:

```
CONTAINER ID   IMAGE     PORTS                  NAMES
b4d12a3cdce5   httpd     0.0.0.0:9000->80/tcp   httpd-web-container
```

Open in browser:

* `http://localhost:9000`
* `http://localhost:8800`

---

## **STEP 7: Container Logs**

### 🧩 View Logs

```bash
docker logs nginx-web-container
docker logs httpd-web-container
```

### 🧩 Follow Logs in Real Time

```bash
docker logs -f nginx-web-container
```

### 🧩 View Last 10 Lines

```bash
docker logs --tail 10 nginx-web-container
```

✅ Open the web page again and see logs update live.

---

## **STEP 8: Copy Files Between Host and Container**

### 🧩 Prepare a File

```bash
echo "<h1>Hello from Docker</h1>" > index.html
```

### 🧩 Copy to Container

```bash
docker cp index.html nginx-web-container:/usr/share/nginx/html/
docker cp index.html httpd-web-container:/usr/local/apache2/htdocs/
```

### 🧩 Verify Inside Container

```bash
docker exec -it nginx-web-container bash
ls /usr/share/nginx/html/
exit
```

### 🧩 Copy Back to Host

```bash
docker cp nginx-web-container:/usr/share/nginx/html/index.html /opt/
```

✅ Check `/opt/index.html` exists on your host.

---

## **STEP 9: Commit Container Changes**

### 🧩 Modify Running Container

```bash
docker exec -it nginx-web-container bash
echo "<h1>Custom Page</h1>" > /usr/share/nginx/html/custom.html
exit
```

### 🧩 Commit Changes as New Image

```bash
docker commit nginx-web-container nginx-custom:v1
```

### 🧩 Tag Image

```bash
docker tag nginx-custom:v1 myrepo/nginx:v1
docker images
```

✅ New image should appear in the list.

---

## **STEP 10: Save and Load Image**

### 🧩 Save to TAR File

```bash
docker save nginx-custom:v1 -o mynginx.tar
ls -lh mynginx.tar
```

### 🧩 Remove Image and Reload

```bash
docker rmi nginx-custom:v1
docker load -i mynginx.tar
```

✅ Check again:

```bash
docker images
```

Your image should reappear.

---

## **STEP 11: Cleanup**

When done:

```bash
docker stop $(docker ps -q)
docker rm $(docker ps -a -q)
docker rmi $(docker images -q)
```

---
