GitHub link - 
### Step 1: Introduction to Kubernetes Namespaces

**What are namespaces?**

Namespaces in Kubernetes provide a mechanism to divide a single cluster into multiple virtual clusters. They allow for separating the resources and objects (like pods, services, and deployments) within a Kubernetes cluster, enabling better organization, isolation, and management of resources.

**Why are namespaces important?**

- **Isolation**: Resources in different namespaces are isolated from each other, meaning you can avoid naming conflicts and prevent unwanted interactions between different environments (e.g., dev, test, prod).
- **Resource Organization**: You can group resources logically based on their environment, application, or ownership.
- **Security**: Role-Based Access Control (RBAC) can be applied at the namespace level, allowing fine-grained access control.
- **Default Namespace**: If a namespace is not specified, resources are created in the `default` namespace. Kubernetes also creates other system namespaces like `kube-system`, which contains control plane components.

### Step 2: Viewing Existing Namespaces

By default, Kubernetes comes with a few namespaces. You can list them using:

```bash
kubectl get namespaces
```

Example output:
```bash
NAME              STATUS   AGE
default           Active   1d
kube-node-lease   Active   1d
kube-public       Active   1d
kube-system       Active   1d
```

- **default**: The default namespace where resources are created if no namespace is specified.
- **kube-system**: Contains system-related components (like `kube-dns`, `kube-proxy`).
- **kube-public**: A public namespace that is readable by everyone, used mainly for cluster information.
- **kube-node-lease**: Tracks the heartbeats of nodes.

### Step 3: Creating a New Namespace (Declarative Way)

A namespace can be created either imperatively or declaratively.

Let's first create a namespace declaratively using a YAML file.

1. Create a `namespace.yaml` file with the following content:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: demo
```

2. Apply the YAML file to create the namespace:

```bash
kubectl apply -f namespace.yaml
```

Output:
```bash
namespace/demo created
```

3. Verify the namespace was created:

```bash
kubectl get namespaces
```

You should see the newly created namespace `demo`.

### Step 4: Creating a Namespace (Imperative Way)

You can also create a namespace directly using a simple command without a YAML file:

```bash
kubectl create namespace demo-imp
```

Output:
```bash
namespace/demo-imp created
```

Again, verify:

```bash
kubectl get namespaces
```

Both `demo` and `demo-imp` namespaces should now be listed.

### Step 5: Deploying Resources to Specific Namespaces

Let’s create a Deployment in the newly created `demo` namespace. By default, resources are created in the `default` namespace unless specified otherwise. We will use the `--namespace` or `-n` flag to target the `demo` namespace.

1. Create an NGINX deployment in the `demo` namespace:

```bash
kubectl create deployment nginx-demo --image=nginx --namespace=demo
```

Output:
```bash
deployment.apps/nginx-demo created
```

2. Verify the deployment:

```bash
kubectl get deployments --namespace=demo
```

Output:
```bash
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx-demo   1/1     1            1           10s
```

3. Check the running pods in the `demo` namespace:

```bash
kubectl get pods --namespace=demo
```

Output:
```bash
NAME                          READY   STATUS    RESTARTS   AGE
nginx-demo-5d8fbbf94d-5htgj   1/1     Running   0          12s
```

### Step 6: Accessing Resources in a Namespace

Resources within the same namespace can communicate with each other directly by their name. However, for cross-namespace communication, the **Fully Qualified Domain Name (FQDN)** must be used.

1. Get the Pod IP for the NGINX pod in the `demo` namespace:

```bash
kubectl get pod -o wide --namespace=demo
```

Output:
```bash
NAME                          READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-demo-5d8fbbf94d-5htgj   1/1     Running   0          1m    10.244.0.5    minikube   <none>           <none>
```

The IP of the pod is `10.244.0.5`.

2. Let’s create another NGINX deployment in the `default` namespace:

```bash
kubectl create deployment nginx-default --image=nginx
```

Verify the pods:

```bash
kubectl get pods --namespace=default
```

Output:
```bash
NAME                              READY   STATUS    RESTARTS   AGE
nginx-default-6b76b5b8ff-7txdh    1/1     Running   0          10s
```

### Step 7: Testing Communication Between Namespaces

Let’s check if the `nginx-default` pod in the `default` namespace can communicate with the `nginx-demo` pod in the `demo` namespace.

1. Exec into the `nginx-default` pod in the `default` namespace:

```bash
kubectl exec -it nginx-default-6b76b5b8ff-7txdh --namespace=default -- /bin/sh
```

Now, from within this shell, try to `curl` the IP of the NGINX pod in the `demo` namespace:

```bash
curl 10.244.0.5
```

Output:
```bash
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and working. Further configuration is required.</p>
</body>
</html>
```

The communication is successful because we used the Pod IP. However, direct DNS-based communication between namespaces requires FQDN.

2. Exit the pod shell:

```bash
exit
```

### Step 8: Deleting a Namespace

To delete a namespace, simply use:

```bash
kubectl delete namespace demo
```

Output:
```bash
namespace "demo" deleted
```

This will remove all the resources within the namespace as well.

### Step 9: Cleaning Up Resources

To clean up all the resources created in this demo, delete the `nginx-default` deployment from the `default` namespace and any other resources you created:

```bash
kubectl delete deployment nginx-default
kubectl delete namespace demo-imp
```

