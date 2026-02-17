# Kubernetes Deployment

**Deployment** provides declarative updates for Pods and ReplicaSets.

**Key points:**
- Higher-level abstraction over ReplicaSet
- Manages ReplicaSets automatically
- Enables rolling updates and rollbacks
- Most common way to deploy applications
- API version: `apps/v1`

---

## **YAML:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: nginx
        image: nginx:1.19
```

---

## **What it does:**
- Creates ReplicaSet automatically
- Manages pod replicas
- Rolling updates (zero-downtime deployments)
- Rollback to previous versions
- Pause and resume deployments
- Scale up/down

---

## **Update Strategies:**

**1. Rolling Update (default):**
- Gradually replaces old pods with new ones
- `maxUnavailable` - max pods down during update
- `maxSurge` - max extra pods during update

**2. Recreate:**
- Kills all old pods first, then creates new ones
- Causes downtime

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

---

## **Common Commands:**

**Create/Update:**
- `kubectl create -f deployment.yaml`
- `kubectl apply -f deployment.yaml`

**View:**
- `kubectl get deployments`
- `kubectl describe deployment myapp-deployment`
- `kubectl rollout status deployment/myapp-deployment`

**Scale:**
- `kubectl scale deployment myapp-deployment --replicas=5`

**Update image:**
- `kubectl set image deployment/myapp-deployment nginx=nginx:1.20`

**Rollout history:**
- `kubectl rollout history deployment/myapp-deployment`
- `kubectl rollout history deployment/myapp-deployment --revision=2`

**Rollback:**
- `kubectl rollout undo deployment/myapp-deployment`
- `kubectl rollout undo deployment/myapp-deployment --to-revision=2`

**Pause/Resume:**
- `kubectl rollout pause deployment/myapp-deployment`
- `kubectl rollout resume deployment/myapp-deployment`

---

## **Deployment → ReplicaSet → Pods hierarchy:**
```
Deployment
    └── ReplicaSet (v1)
            ├── Pod 1
            ├── Pod 2
            └── Pod 3
```

When you update, it creates a new ReplicaSet and scales down the old one.