# YAML in Kubernetes

**YAML in Kubernetes** is used to define and configure Kubernetes objects.

---

## **Structure - 4 Required Top-Level Fields:**

```yaml
apiVersion: v1           # API version
kind: Pod               # Type of object
metadata:               # Data about the object
  name: my-pod
  labels:
    app: myapp
spec:                   # Desired state/configuration
  containers:
  - name: nginx
    image: nginx
```

---

## **Key Sections:**

### **1. apiVersion:**
- `v1` - Pod, Service, ConfigMap, Secret
- `apps/v1` - Deployment, ReplicaSet, StatefulSet, DaemonSet
- `batch/v1` - Job, CronJob
- `networking.k8s.io/v1` - NetworkPolicy, Ingress

### **2. kind:**
- Pod, Service, Deployment, ReplicaSet, etc.

### **3. metadata:**
- `name` - object name (required)
- `namespace` - namespace location
- `labels` - key-value pairs for organization
- `annotations` - non-identifying metadata

### **4. spec:**
- Desired state and configuration
- Different for each object type

---

## **Common Commands:**
- `kubectl create -f file.yaml` - create from file
- `kubectl apply -f file.yaml` - create or update
- `kubectl delete -f file.yaml` - delete
- `kubectl get pod my-pod -o yaml` - view as YAML

---

## **Tips:**
- Indentation matters (use spaces, not tabs)
- Use `---` to separate multiple objects in one file
- `kubectl explain pod.spec` - see field documentation