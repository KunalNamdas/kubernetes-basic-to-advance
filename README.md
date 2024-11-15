# **Comprehensive Guide to Kubernetes: From Basics to Advanced**

---

## **Chapter 1: Introduction to Kubernetes**

### **What is Kubernetes?**
Kubernetes is an open-source container orchestration platform developed by Google. It automates the deployment, scaling, and management of containerized applications.

### **Why Use Kubernetes?**
- **Scalability**: Automatically scale applications up or down.
- **High Availability**: Ensures applications remain available even in case of failures.
- **Portability**: Runs on various environments (on-premise, cloud, hybrid).
- **Resource Efficiency**: Optimizes the use of infrastructure resources.

---

## **Chapter 2: Kubernetes Architecture**

### **Key Components**:
1. **Master Node**:
   - **API Server**: Acts as the control plane and interface for Kubernetes.
   - **Controller Manager**: Ensures the desired state of the system.
   - **Scheduler**: Assigns workloads to nodes.

2. **Worker Nodes**:
   - **Kubelet**: Manages containers on a node.
   - **Kube-proxy**: Handles networking for pods.
   - **Container Runtime**: Manages container lifecycle (e.g., Docker, CRI-O).

3. **Other Components**:
   - **etcd**: A key-value store for configuration and state.
   - **Pods**: Smallest deployable unit in Kubernetes.

---

## **Chapter 3: Installing Kubernetes**

### **Methods of Installation**:
1. **Minikube** (For local setups):
   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   minikube start
   ```

2. **Kubernetes on Cloud**:
   - Use managed Kubernetes services like **Amazon EKS**, **Google GKE**, or **Azure AKS**.

3. **Kubeadm** (For production clusters):
   ```bash
   sudo apt-get update
   sudo apt-get install -y kubeadm kubelet kubectl
   kubeadm init
   ```

---

## **Chapter 4: Kubernetes Objects**

### **Core Objects**:
- **Pods**: Group of containers sharing resources.
- **Services**: Exposes a set of pods as a network service.
- **Namespaces**: Logical partitions for resources.

### **Other Objects**:
- **ConfigMaps**: Store configuration data.
- **Secrets**: Manage sensitive data.
- **Volumes**: Handle persistent storage.

---

## **Chapter 5: Writing Your First Kubernetes Manifest**

### **Example Pod YAML**:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

### **1. `apiVersion: v1`**
- **What It Means**: Specifies the version of the Kubernetes API you’re using.  
- **Why It’s Needed**: Different API versions have varying capabilities and features. In this case:
  - `v1` indicates this is a core Kubernetes object (like Pods, Services, etc.).

---

### **2. `kind: Pod`**
- **What It Means**: Defines the type of Kubernetes object you want to create.  
- **Why It’s Needed**: Kubernetes uses this to identify that you're creating a `Pod`. Other examples of `kind` include `Service`, `Deployment`, etc.

---

### **3. `metadata:`**
- **What It Means**: Contains metadata for the object, like its name, namespace, and labels.  
- **Why It’s Needed**: Metadata helps in organizing and identifying the object within the Kubernetes cluster.

---

#### **3.1. `name: nginx-pod`**
- **What It Means**: Assigns the Pod a unique name (`nginx-pod`).  
- **Why It’s Needed**: A name is required to refer to the Pod in commands or other configurations.

---

### **4. `spec:`**
- **What It Means**: Specifies the desired state of the object.  
- **Why It’s Needed**: This is where you define details about what the Pod should run, such as the containers it will use and their configurations.

---

#### **4.1. `containers:`**
- **What It Means**: Lists the containers that will run inside the Pod.  
- **Why It’s Needed**: Kubernetes manages containers, and you need to define at least one container for the Pod to run.

---

##### **4.1.1. `- name: nginx`**
- **What It Means**: Assigns a name (`nginx`) to the container inside the Pod.  
- **Why It’s Needed**: Naming containers helps when managing logs, troubleshooting, and referencing specific containers in commands.

---

##### **4.1.2. `image: nginx:latest`**
- **What It Means**: Specifies the container image to be used for this container.
  - `nginx`: Refers to the official NGINX image from Docker Hub.
  - `latest`: Specifies the latest version of the image.
- **Why It’s Needed**: Kubernetes pulls this image from the container registry to create the container.

---

### **Commands to Apply**:
1. Apply the manifest:
   ```bash
   kubectl apply -f pod.yaml
   ```
2. View the pod:
   ```bash
   kubectl get pods
   ```

---

## **Chapter 6: Kubernetes Services**

Services expose your pods to other applications or external users.

### **Types of Services**:
- **ClusterIP**: Default, accessible within the cluster.
- **NodePort**: Exposes service on a port on each node.
- **LoadBalancer**: Integrates with cloud provider load balancers.

### **Service Example**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```

---

## **Chapter 7: Scaling Applications in Kubernetes**

### **Horizontal Pod Autoscaler**:
Automatically scales pods based on CPU/memory usage.
```bash
kubectl autoscale deployment my-app --cpu-percent=50 --min=1 --max=10
```

### **Manual Scaling**:
```bash
kubectl scale deployment my-app --replicas=5
```

---

## **Chapter 8: Kubernetes Networking**

### **Networking Basics**:
- **Pod-to-Pod Communication**: Pods can communicate across nodes.
- **Service-to-Pod Communication**: Services route traffic to pods.

### **Ingress**:
Ingress exposes HTTP and HTTPS routes to services.
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

---

## **Chapter 9: Persistent Storage in Kubernetes**

Kubernetes uses **Persistent Volumes (PVs)** and **Persistent Volume Claims (PVCs)** for storage.

### **Example**:
1. **Persistent Volume**:
   ```yaml
   apiVersion: v1
   kind: PersistentVolume
   metadata:
     name: pv-example
   spec:
     capacity:
       storage: 10Gi
     accessModes:
       - ReadWriteOnce
     hostPath:
       path: /data
   ```

2. **Persistent Volume Claim**:
   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: pvc-example
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 5Gi
   ```

---

## **Chapter 10: Helm: Kubernetes Package Manager**

Helm simplifies application deployment on Kubernetes.

### **Installing Helm**:
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### **Deploying with Helm**:
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-release bitnami/nginx
```

---

## **Chapter 11: Kubernetes Security**

### **Best Practices**:
1. **RBAC (Role-Based Access Control)**:
   - Assign permissions to users and services.
2. **Network Policies**:
   - Restrict communication between pods.
3. **Secrets**:
   - Securely store sensitive data like passwords.

### **Example Network Policy**:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app
spec:
  podSelector:
    matchLabels:
      app: MyApp
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: Frontend
```

---

## **Chapter 12: Kubernetes Monitoring and Logging**

### **Monitoring Tools**:
- **Prometheus**: Collects metrics from Kubernetes.
- **Grafana**: Visualizes metrics.
- **Kubernetes Dashboard**: Web UI to manage resources.

### **Logging Tools**:
- **Fluentd**: Aggregates logs.
- **Elasticsearch and Kibana**: For log storage and visualization.

---

## **Chapter 13: Kubernetes Commands and Their Usage**

### **Core Commands**:
- **kubectl get pods**: List all pods.
- **kubectl describe pod [POD_NAME]**: View detailed information about a pod.
- **kubectl delete pod [POD_NAME]**: Delete a pod.
- **kubectl logs [POD_NAME]**: View logs from a pod.

### **Advanced Commands**:
- **kubectl exec [POD_NAME] -- [COMMAND]**: Execute a command inside a pod.
- **kubectl port-forward [POD_NAME] 8080:80**: Forward a port from your local machine to a pod.
- **kubectl apply -f [FILE_NAME]**: Apply a manifest file.
- **kubectl rollout restart deployment [DEPLOYMENT_NAME]**: Restart a deployment.

---

## **Chapter 14: Advanced Kubernetes Features**

### **1. StatefulSets**:
Manages stateful applications with unique identities.
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
```

### **2. Custom Resource Definitions (CRDs)**:
Extend Kubernetes functionalities by defining new resources.

---

## **Conclusion**

Kubernetes is an essential tool for modern application development and management. This guide provides a structured roadmap for learning Kubernetes, from installation to advanced features.

**Authored by Kunal Namdas**
