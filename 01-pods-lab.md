
---

## 🧪 **Kubernetes Pod Lab Exercise**

### **Objective:**

Practice basic `kubectl` commands to manage Pods.

---

### **Step 1: Create a Pod**

Create a simple Pod YAML file named `my-pod.yaml`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: nginx
    env: dev
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

👉 **Apply the YAML:**

```bash
kubectl apply -f my-pod.yaml
```

---

### **Step 2: List Pods**

List all Pods in the current namespace.

```bash
kubectl get pods
```

List Pods with detailed (wide) information (shows Node name, IP, etc.):

```bash
kubectl get pods -o wide
```

List Pods with their **labels:**

```bash
kubectl get pods --show-labels
```

---

### **Step 3: Filter Pods Using Labels**

Get Pods with a specific label:

```bash
kubectl get pods -l app=nginx
```

Add a new label to the Pod:

```bash
kubectl label pod my-pod version=v1
```

Verify:

```bash
kubectl get pods --show-labels
```

---

### **Step 4: Describe the Pod**

View detailed information about the Pod (events, containers, IP, etc.):

```bash
kubectl describe pod my-pod
```

---

### **Step 5: View Pod Logs**

Check the logs of the container running inside the Pod:

```bash
kubectl logs my-pod
```

If there are multiple containers (example: `nginx-container`), specify it:

```bash
kubectl logs my-pod -c nginx-container
```

---

### **Step 6: Access Pod (Optional)**

Run a command inside the Pod (e.g., check Nginx default page):

```bash
kubectl exec -it my-pod -- /bin/bash
```

Then run:

```bash
curl localhost
exit
```

---

### **Step 7: Delete the Pod**

Delete the Pod using either command:

```bash
kubectl delete pod my-pod
```

Or delete from YAML:

```bash
kubectl delete -f my-pod.yaml
```

---

### ✅ **Lab Validation**

* [ ] Pod created successfully
* [ ] Pod visible in `kubectl get pods -o wide`
* [ ] Labels added and verified
* [ ] Pod described successfully
* [ ] Logs retrieved successfully
* [ ] Pod deleted successfully

---
