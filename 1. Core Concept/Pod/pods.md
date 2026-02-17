**Pod** is the smallest deployable unit in Kubernetes.

**Key points:**
- Single instance of a running process
- Can contain one or more containers (usually one)
- Containers in a pod share network and storage
- Ephemeral - pods are disposable and replaceable
- Each pod gets a unique IP address

- Containers share same network namespace (localhost communication)
- Containers share same storage volumes
- Scheduled together on same node
- Atomic unit - deployed/deleted together

---

## **Pod Lifecycle States:**
- **Pending** - accepted but not yet running
- **Running** - bound to node, containers created
- **Succeeded** - all containers terminated successfully
- **Failed** - containers terminated with errors
- **Unknown** - state cannot be determined

---

## **YAML Structure:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx
    image: nginx
```

---

## **Common Patterns:**
- **Single container** - most common
- **Multi-container** - sidecar, ambassador, adapter patterns
- **Init containers** - run before main containers

---

## **Common Commands:**
- `kubectl run` - create pod
- `kubectl get pods` - list pods
- `kubectl describe pod` - details
- `kubectl delete pod` - delete
- `kubectl logs` - view logs
- `kubectl exec` - execute commands

**Note:** Pods are usually managed by controllers (Deployment, ReplicaSet, StatefulSet) not created directly.