# Kubernetes Services

**Services** enable network access to a set of pods.

**Key points:**
- Provides stable IP address and DNS name for pods
- Load balances traffic across pod replicas
- Pods are ephemeral, Services are stable
- Uses selectors to find target pods
- API version: `v1`

---

## **Service Types:**

### **1. ClusterIP (default)**
- Exposes service on internal cluster IP
- Only accessible within the cluster
- Used for internal communication

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: myapp
  ports:
  - port: 80          # Service port
    targetPort: 8080  # Container port
```

### **2. NodePort**
- Exposes service on each node's IP at a static port
- Accessible from outside cluster via `<NodeIP>:<NodePort>`
- Port range: 30000-32767

```yaml
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30008   # Optional, auto-assigned if not specified
```

### **3. LoadBalancer**
- Creates external load balancer (cloud provider)
- Assigns external IP
- Includes NodePort and ClusterIP functionality

```yaml
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 8080
```

### **4. ExternalName**
- Maps service to DNS name
- No proxying, returns CNAME record

```yaml
spec:
  type: ExternalName
  externalName: my.database.example.com
```

---

## **Full YAML Example:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp        # Matches pods with this label
  ports:
  - protocol: TCP
    port: 80          # Service port
    targetPort: 8080  # Pod port
  type: ClusterIP
```

---

## **How it works:**
- Service gets ClusterIP (e.g., `10.96.0.10`)
- Selects pods with matching labels
- kube-proxy creates iptables/ipvs rules
- Traffic to ClusterIP is forwarded to pods
- DNS entry: `<service-name>.<namespace>.svc.cluster.local`

---

## **Commands:**
- `kubectl get services` / `kubectl get svc`
- `kubectl describe service myapp-service`
- `kubectl expose deployment myapp --port=80 --target-port=8080`
- `kubectl delete service myapp-service`

---

## **Service Discovery:**
- **Environment variables** - injected into pods
- **DNS** - `my-service.default.svc.cluster.local`

---

## **Port Types:**
- **port** - Service port (where service is exposed)
- **targetPort** - Container/pod port (where app listens)
- **nodePort** - External port on node (for NodePort type)