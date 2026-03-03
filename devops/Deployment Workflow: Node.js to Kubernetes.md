### **Deployment Workflow: Node.js to Kubernetes**

**Node.js App Local Development**  
⬇️  
**Dockerization** (Create `Dockerfile` & Build Image)  
⬇️  
**Docker Hub** (Tag & Push Image to Registry)  
⬇️  
**Kubernetes Environment** (Start Minikube Cluster)  
⬇️  
**Orchestration** (Create & Apply Deployment and Service YAMLs)  
⬇️  
**Scaling & Access** (Deploy 3 Pods & Expose via Load Balancer)

---

### **1. Local Node.js Application Setup**
*   The tutorial uses a simple **Express server** that listens on **port 3000** and returns a "Hello from Node.js app running in Kubernetes" message.
*   Ensure your `package.json` includes the necessary dependencies (e.g., Express) and a start script.

### **2. Dockerizing the Application**
To containerize the app, you need a **Dockerfile** and a **.dockerignore** file in your project root.

*   **.dockerignore**: Prevents bulky or sensitive files like `node_modules` and `npm-debug.log` from being built into the image.
*   **Dockerfile Configuration**:
    *   **Base Image**: `FROM node:22-Alpine`
    *   **Working Directory**: `WORKDIR /user/source/app`
    *   **Install Dependencies**: Copy `package.json` first and run `npm install` to optimize caching.
    *   **Copy Code**: Copy the rest of the application files.
    *   **Expose Port**: `EXPOSE 3000`
    *   **Start Command**: `CMD ["node", "index.js"]`

**Key Commands:**
*   **Build Image**: `docker build -t <app-name> .`
*   **Run Locally**: `docker run -p 3000:3000 -d <app-name>` (Runs in detached mode mapping host port 3000 to container port 3000).

### **3. Pushing to Docker Hub**
Kubernetes needs to pull your image from a registry like **Docker Hub**.

1.  **Login**: `docker login -u <username>`.
2.  **Tag**: `docker tag <app-name> <username>/<app-name>:1.0`.
3.  **Push**: `docker push <username>/<app-name>:1.0`.

### **4. Kubernetes Deployment with Minikube**
**Minikube** is used as a local Kubernetes cluster for development. Start it using `minikube start`.

#### **A. The Deployment YAML**
This file defines the desired state of your application (how many instances/pods to run).
*   **Generation**: You can generate a template using:  
    `kubectl create deployment <name> --image=<username>/<app-name>:1.0 --dry-run=client -o yaml > deployment.yaml`.
*   **Configuration**: Edit the file to set **`replicas: 3`** for high availability and define the **`containerPort: 3000`**.
*   **Apply**: `kubectl apply -f deployment.yaml`.

#### **B. The Service YAML**
A **Service** provides a stable endpoint (Load Balancer) to access the pods.
*   **Generation**: `kubectl expose deployment <name> --port=80 --target-port=3000 --type=LoadBalancer --dry-run=client -o yaml > service.yaml`.
*   **Apply**: `kubectl apply -f service.yaml`.

### **5. Monitoring and Access**
Once applied, Kubernetes manages the resource creation.

*   **Check Resources**:
    *   `kubectl get pods`: Shows the status of the three running pods.
    *   `kubectl get all`: Provides a comprehensive view of pods, services, and deployments.
*   **Logs & Troubleshooting**:
    *   **View Logs**: `kubectl logs <pod-name>`.
    *   **Detailed Status**: `kubectl describe deployment <name>` or `kubectl describe service <name>`.
*   **Access the App**: Since it is running on a local cluster with a Load Balancer, use Minikube to fetch the external URL:  
    `minikube service <service-name> --url`.

Would you like me to create a set of flashcards to help you memorize these Kubernetes and Docker commands?
