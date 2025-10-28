
---

## 🧪 **Docker Lab Exercise — Docker Network Management**

### **🎯 Objective**

You will learn how to:

* Create custom Docker bridge networks with subnet and gateway
* Attach containers to networks
* Validate network and IP assignments
* Connect and disconnect containers from networks
* Inspect and remove networks

---

### **🧩 Step 1: List existing networks**

```bash
docker network ls
```

🔍 Check default networks (`bridge`, `host`, `none`) and note that no custom ones exist yet.

---

### **🧩 Step 2: Create a custom network with subnet and gateway**

```bash
docker network create \
  --driver bridge \
  --subnet 192.168.50.0/24 \
  --gateway 192.168.50.1 \
  app_net1
```

✅ Creates a new network called **`app_net1`**.

---

### **🧩 Step 3: Inspect the network**

```bash
docker network inspect app_net1
```

📋 Verify subnet, gateway, and confirm there are no containers attached yet.

---

### **🧩 Step 4: Create a container in the new network**

```bash
docker run -d --name web1 --network app_net1 nginx
```

✅ Runs an Nginx container attached to `app_net1`.

---

### **🧩 Step 5: Validate container IP and network**

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web1
```

or

```bash
docker exec -it web1 ip addr show eth0
```

📍 You should see an IP from `192.168.50.0/24`.

---

### **🧩 Step 6: Create another container (not connected yet)**

```bash
docker run -d --name web2 nginx
```

✅ Runs `web2` in the **default bridge** network.

---

### **🧩 Step 7: Create another network**

```bash
docker network create \
  --driver bridge \
  --subnet 10.10.10.0/24 \
  --gateway 10.10.10.1 \
  app_net2
```

✅ Creates **`app_net2`**.

---

### **🧩 Step 8: Connect existing container (web1) to new network**

```bash
docker network connect app_net2 web1
```

✅ Now `web1` is attached to both `app_net1` and `app_net2`.

---

### **🧩 Step 9: Inspect container to verify multiple networks**

```bash
docker inspect web1 | grep -A5 Networks
```

🔍 You’ll see both `app_net1` and `app_net2` listed, each with unique IPs.

---

### **🧩 Step 10: Disconnect container from one network**

```bash
docker network disconnect app_net1 web1
```

✅ Removes `web1` from `app_net1`.

---

### **🧩 Step 11: Validate disconnection**

```bash
docker inspect web1 | grep -A5 Networks
```

📋 You should now see only `app_net2`.

---

### **🧩 Step 12: Inspect and remove networks**

Before removing, make sure no container is connected:

```bash
docker network inspect app_net1
docker network inspect app_net2
```

If no containers remain:

```bash
docker network rm app_net1 app_net2
```

🧹 Cleans up the custom networks.

---

### **🧩 Step 13: (Optional) Validate network communication**

You can check connectivity between containers on the same network.

For example:

```bash
docker exec -it web1 ping -c 3 web2
```

If they’re on **different networks**, ping will fail — showing isolation.

---

### ✅ **Summary of Key Commands**

| Purpose                              | Command Example                                   |
| ------------------------------------ | ------------------------------------------------- |
| List networks                        | `docker network ls`                               |
| Create network                       | `docker network create --subnet --gateway`        |
| Inspect network                      | `docker network inspect <name>`                   |
| Run container in network             | `docker run --network <name>`                     |
| Connect container to another network | `docker network connect <network> <container>`    |
| Disconnect container from network    | `docker network disconnect <network> <container>` |
| Remove network                       | `docker network rm <network>`                     |

---
