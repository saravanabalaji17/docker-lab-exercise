
---

## 🧪 **Docker Lab Exercise — Manage Docker Networks and Containers**

### **🎯 Objective**

You will learn to:

* Create Docker networks with custom subnet and gateway
* Run containers on specific networks
* Inspect and validate IPs and network configuration
* Connect/disconnect containers from networks
* Remove networks safely

---

### **🧩 Step 1: Check existing Docker networks**

```bash
docker network ls
```

👉 Lists the default networks: `bridge`, `host`, `none`.

---

### **🧩 Step 2: Create the first custom network**

```bash
docker network create \
  --driver bridge \
  --subnet 192.168.100.0/24 \
  --gateway 192.168.100.1 \
  net1
```

✅ Creates a network named **net1** with a custom subnet and gateway.

---

### **🧩 Step 3: Inspect the network**

```bash
docker network inspect net1
```

📋 Verify the subnet, gateway, and ensure no containers are attached yet.

---

### **🧩 Step 4: Run a container in the new network**

```bash
docker run -d --name container1 --network net1 nginx
```

✅ Launches a container called **container1** in **net1**.

---

### **🧩 Step 5: Validate container network and IP**

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container1
```

or inside the container:

```bash
docker exec -it container1 ip addr show eth0
```

📍 Confirm that the IP belongs to `192.168.100.0/24`.

---

### **🧩 Step 6: Create another custom network**

```bash
docker network create \
  --driver bridge \
  --subnet 10.20.30.0/24 \
  --gateway 10.20.30.1 \
  net2
```

✅ Creates a second network named **net2**.

---

### **🧩 Step 7: Run another container in the new network**

```bash
docker run -d --name container2 --network net2 nginx
```

✅ Creates **container2** attached to **net2**.

---

### **🧩 Step 8: Inspect both networks**

```bash
docker network inspect net1
docker network inspect net2
```

🔍 Validate which containers are connected to each network.

---

### **🧩 Step 9: Connect an existing container (container1) to the second network**

```bash
docker network connect net2 container1
```

✅ Now `container1` is attached to **both** networks (`net1` and `net2`).

---

### **🧩 Step 10: Validate multi-network connection**

```bash
docker inspect container1 | grep -A5 Networks
```

🔍 You’ll see both `net1` and `net2` with different IPs assigned.

---

### **🧩 Step 11: Disconnect container from one network**

```bash
docker network disconnect net1 container1
```

✅ Removes `container1` from **net1**.

---

### **🧩 Step 12: Validate disconnection**

```bash
docker inspect container1 | grep -A5 Networks
```

📋 Now only `net2` should remain for `container1`.

---

### **🧩 Step 13: Remove networks**

Before removing, ensure containers are disconnected:

```bash
docker network rm net1 net2
```

🧹 Removes both custom networks.

---

### **🧩 Step 14: Optional — Test communication between containers**

To verify connectivity:

```bash
docker exec -it container2 ping -c 3 container1
```

💡 Ping works only if both containers share the same network.

---

### ✅ **Summary of Commands**

| Action                            | Command                                           |
| --------------------------------- | ------------------------------------------------- |
| List networks                     | `docker network ls`                               |
| Create network                    | `docker network create --subnet --gateway`        |
| Inspect network                   | `docker network inspect <network>`                |
| Run container in network          | `docker run --network <network>`                  |
| Connect container to network      | `docker network connect <network> <container>`    |
| Disconnect container from network | `docker network disconnect <network> <container>` |
| Remove network                    | `docker network rm <network>`                     |

---
