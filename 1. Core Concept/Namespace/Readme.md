# Kubernetes Namespace

**Namespace** provides logical isolation and organization within a cluster.

**Key points:**
- Virtual clusters within a physical cluster
- Isolates resources between teams/projects/environments
- Resource quotas and limits per namespace
- Most resources are namespaced (pods, services, deployments)
- Some resources are cluster-wide (nodes, PV, namespace itself)
- API version: `v1`

---

## **Default Namespaces:**

1. **default** - default namespace for objects with no namespace specified
2. **kube-system** - for Kubernetes system components (DNS, dashboard, etc.)
3. **kube-public** - publicly accessible, readable by all users
4. **kube-node-lease** - node heartbeat/lease objects for node health

---

## **YAML:**
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

---

## **Commands:**

**Create:**
- `kubectl create namespace dev`
- `kubectl create -f namespace.yaml`

**List:**
- `kubectl get namespaces` / `kubectl get ns`

**View resources in namespace:**
- `kubectl get pods --namespace=dev` / `kubectl get pods -n dev`
- `kubectl get all -n dev`

**Set default namespace:**
- `kubectl config set-context --current --namespace=dev`

**Delete:**
- `kubectl delete namespace dev` (deletes all resources in it)

---

## **Using Namespaces in YAML:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: dev     # Specify namespace
spec:
  containers:
  - name: nginx
    image: nginx
```

---

## **Resource Quotas:**
Limit resources per namespace:
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
```

---

## **DNS in Namespaces:**
Services can be accessed via:
- Same namespace: `service-name`
- Different namespace: `service-name.namespace-name`
- Full: `service-name.namespace-name.svc.cluster.local`

Example: `db-service.prod.svc.cluster.local`

---

## **Use Cases:**
- **Environments**: dev, staging, prod
- **Teams**: team-a, team-b
- **Projects**: project-x, project-y
- **Resource isolation and quotas**

---

## **What's NOT Namespaced:**
- Nodes
- PersistentVolumes
- Namespaces themselves
- ClusterRoles, ClusterRoleBindings

Check: `kubectl api-resources --namespaced=false`