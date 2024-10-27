# Intro to Kubernetes!

## Agenda
* Part 1: Containers & Docker 
* Part 2: Kubernetes 
* Part 3: Key Kubernetes Components
* Part 4: Hands-On Demo
* Part 5: Wrap-Up and Q&A

## Slides

Link to [slides](https://github.com/savitharaghunathan/101-kubernetes/blob/main/Introduction%20to%20Kubernetes!.pdf) 

## Demo Guide

### Step 1: Create a Cluster

1. **Create a Kind Cluster** using the Kind CLI:

    ```bash
    kind create cluster --config 00-create-cluster.yaml
    ```

   - This command creates a new Kind cluster named. Once created, you can verify the cluster by listing nodes:

    ```bash
    kubectl get nodes
    ```

2. **Check Cluster Status** to ensure that it’s up and running:

    ```bash
    kubectl cluster-info
    ```

---

### Step 2: Create an Nginx Deployment

1. **Apply the Nginx Deployment Manifest** from your GitHub repository:

    ```bash
    kubectl apply -f 01-create-deployment.yaml
    ```

   - This creates a Deployment named `nginx-deployment`, which will manage the lifecycle of the Nginx Pods.

2. **Verify the Deployment** by listing Pods:

    ```bash
    kubectl get pods -l app=nginx
    ```

   - The output should show one or more Pods in a `Running` state, as defined by the Deployment configuration.

---

### Step 3: Create a Service to Expose Nginx

1. **Apply the Service Manifest** to expose the Nginx Deployment:

    ```bash
    kubectl apply -f 02-create-service.yaml
    ```

   - This creates a Service of type `NodePort`, which exposes the Nginx Deployment on a specified port, allowing external access.

2. **Check the Service** to verify that it’s created and note the `NodePort`:

    ```bash
    kubectl get svc nginx-service
    ```

   - The output should include a `NodePort` for external access. For example, if the `NodePort` is `30007`, you can access Nginx at `localhost:30007`.

3. **Alternative Access using Port Forwarding**:

    If accessing `NodePort` fails, use port forwarding:

    ```bash
    kubectl port-forward svc/nginx-service 30007:80
    ```

   - You can now access Nginx at `localhost:30007`.

---

### Step 4: Scale the Deployment

1. **Scale the Nginx Deployment** to 5 replicas:

    ```bash
    kubectl scale deployment nginx-deployment --replicas=5
    ```

   - This command updates the Deployment to maintain 5 replicas of the Nginx Pods.

2. **Verify Scaling** by checking the number of running Pods:

    ```bash
    kubectl get pods -l app=nginx
    ```

   - You should see 5 Pods in a `Running` state.

---

### Step 5: Delete a Pod and Observe Self-Healing

1. **Delete an Nginx Pod** by specifying one of the Pod names:

    ```bash
    kubectl delete pod <POD_NAME>
    ```

   - Replace `<POD_NAME>` with the name of any Nginx Pod from the output of `kubectl get pods`.

2. **Watch Kubernetes Self-Healing**:

    ```bash
    kubectl get pods -l app=nginx -w
    ```

   - The `-w` flag will keep the command running to watch for changes in real-time. You’ll see Kubernetes automatically create a new Pod to replace the deleted one, maintaining the desired replica count of 5.

---

These steps cover creating and managing a Kubernetes cluster, deploying and scaling applications, exposing services, and demonstrating Kubernetes self-healing features.
