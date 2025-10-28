
---

## 🧪 **Lab: Docker Volume Creation & Data Persistence**

### 🎯 **Goal**

Learn how to:

* Create a Docker volume
* Attach it to a container
* Add and validate persistent data
* Remove and recreate containers without losing data

---

### 🧩 **STEP 1 — Verify Docker Setup**

```bash
docker --version
docker info
```

✅ **Checkpoint:**
Ensure Docker daemon is running and you can run basic commands.

---

### 🧩 **STEP 2 — Create a Docker Volume**

```bash
docker volume create mydata
```

List volumes:

```bash
docker volume ls
```

Inspect volume details:

```bash
docker volume inspect mydata
```

📘 **Explanation:**
This creates a managed storage area (under `/var/lib/docker/volumes/mydata/_data`).

---

### 🧩 **STEP 3 — Run a Container with the Volume and Port Mapping**

Run an Nginx container and mount the volume:

```bash
docker run -d --name web1 -p 8080:80 -v mydata:/usr/share/nginx/html nginx
```

✅ **Check running container:**

```bash
docker ps
```

---

### 🧩 **STEP 4 — Login to Container and Add Some Data**

```bash
docker exec -it web1 /bin/bash
```

Inside the container:

```bash
cd /usr/share/nginx/html
echo "Hello from web1 at $(date)" > index.html
exit
```

---

### 🧩 **STEP 5 — Validate Data from Container**

From the **host**, run:

```bash
curl http://localhost:8080
```

✅ You should see:

```
Hello from web1 at <timestamp>
```

---

### 🧩 **STEP 6 — Check Data in Volume (Optional)**

Find volume mount point:

```bash
docker volume inspect mydata | grep Mountpoint
```

Example output:

```
"Mountpoint": "/var/lib/docker/volumes/mydata/_data"
```

Check content directly on host:

```bash
cat /var/lib/docker/volumes/mydata/_data/index.html
```

---

### 🧩 **STEP 7 — Delete the Container**

```bash
docker rm -f web1
```

Check that volume still exists:

```bash
docker volume ls
```

Inspect again:

```bash
docker volume inspect mydata
```

✅ **Volume still remains** even though the container was removed.

---

### 🧩 **STEP 8 — Create Another Container with the Existing Volume**

```bash
docker run -d --name web2 -p 8081:80 -v mydata:/usr/share/nginx/html nginx
```

Now access the new container:

```bash
curl http://localhost:8081
```

✅ You should still see:

```
Hello from web1 at <timestamp>
```

🎉 **Success!**
The data persisted because it was stored in the Docker volume, **not inside the container filesystem**.

---

### 🧩 **STEP 9 — Clean Up**

When you’re done:

```bash
docker rm -f web2
docker volume rm mydata
```

---

### 💡 **Learning Points**

* Containers are **ephemeral** — data inside them is lost when deleted.
* Docker **volumes** provide **persistent storage** beyond container lifetimes.
* Volumes can be shared across multiple containers.

---
