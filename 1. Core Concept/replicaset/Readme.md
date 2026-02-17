# Kubernetes ReplicaSet & Replication Controller

**Replication Controller** (old) and **ReplicaSet** (new) both ensure a specified number of pod replicas are running.

---

## **Replication Controller** (Legacy)

**Key points:**
- Older technology, being replaced by ReplicaSet
- Ensures specified number of pods are running
- Creates new pods if they fail/are deleted
- Uses equality-based selectors only

**YAML:**
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
spec:
  replicas: 3
  selector:
    app: myapp          # Equality-based only
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## **ReplicaSet** (Current)

**Key points:**
- Replacement for Replication Controller
- More powerful selector options (set-based)
- Usually managed by **Deployment** (not created directly)
- Requires `selector` field (unlike RC)

**YAML:**
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
    matchExpressions:    # Set-based selectors
    - key: tier
      operator: In
      values:
      - frontend
  template:
    metadata:
      labels:
        app: myapp
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## **Key Differences:**

| Feature | Replication Controller | ReplicaSet |
|---------|----------------------|------------|
| API Version | v1 | apps/v1 |
| Selector Type | Equality only (`app=myapp`) | Equality + Set-based (`In`, `NotIn`, `Exists`) |
| Current Use | Deprecated | Preferred (via Deployment) |
| Selector Required | No | Yes |

---

## **Commands:**
- `kubectl get replicaset` / `kubectl get rs`
- `kubectl scale rs myapp-rs --replicas=5`
- `kubectl delete rs myapp-rs`

---

## **Best Practice:**
Use **Deployments** instead of creating ReplicaSets directly.