### **Step 1: Install `kind`**

`kind` is a tool that lets you run Kubernetes clusters locally using Docker containers as cluster nodes. To install `kind`, you can use Homebrew (for macOS):

```bash
brew install kind
```

To verify that `kind` is installed successfully, run:

```bash
kind version
```

This should display the version of `kind` installed.

---

### **Step 2: Create a Cluster with `kind`**

Once `kind` is installed, you can create a Kubernetes cluster. By default, `kind` creates a single-node cluster, but you can configure a multi-node cluster. To create a simple single-node cluster:

```bash
kind create cluster --name eka-cluster1
```

For a multi-node cluster, you'll need to provide a configuration file. First, create a YAML configuration file (`config.yml`), which defines the cluster's setup.

Example for a **multi-node** configuration:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane  # This will act as the master node
  - role: worker         # Worker node 1
  - role: worker         # Worker node 2
```

After saving the above configuration to `config.yml`, create the cluster:

```bash
kind create cluster --name eka-cluster1 --config config.yml
```

This command will create a cluster named `eka-cluster1` with one control-plane node and two worker nodes.

---

### **Step 3: Install `kubectl`**

`kubectl` is the command-line tool used to interact with Kubernetes clusters. To install `kubectl`, run the following command:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"
```

 **Ensure that your `kubectl` version is compatible with the Kubernetes version used by `kind`.**

---

### **Step 4: Verify the Cluster is Running**

Once the cluster is created, you can verify that all the nodes (control-plane and worker nodes) are up and running using `kubectl`:

```bash
kubectl get nodes
```

This should show you a list of all the nodes in the cluster, along with their statuses.

Here, we can see that the control-plane and worker nodes are running.

---

### **Step 5: Create a Multi-Node Configuration File**

If you haven’t already, create a custom `kind` configuration file to define your multi-node cluster. This allows you to explicitly control how many nodes (control-plane and workers) are created.

Here’s an example `config.yml`:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane  # This node will run the Kubernetes master components
  - role: worker         # Worker node 1
  - role: worker         # Worker node 2
  - role: worker         # Worker node 3 (optional)
networking:
  disableDefaultCNI: false
```

You can adjust the number of worker nodes as needed. The `disableDefaultCNI: false` ensures that `kind` uses its default CNI (Container Network Interface).

Now, create the cluster with the new configuration:

```bash
kind create cluster --name eka-cluster --config config.yml
```

This will set up a control-plane (master) node and multiple worker nodes.

---

### **Step 6: Inspect the Cluster Context**

Kubernetes supports multiple clusters, and `kubectl` uses contexts to manage these clusters. To view the contexts (active clusters) on your system, run:

```bash
kubectl config get-contexts
```

This will list all the available contexts and indicate which one is currently active.

For example:

```bash
CURRENT   NAME                CLUSTER             AUTHINFO            NAMESPACE
*         kind-eka-cluster    kind-eka-cluster    kind-eka-cluster
```

The `*` indicates the current active context, meaning you are working in the `kind-eka-cluster`.

---

### **Step 7: Switch Contexts **

If you're working with multiple clusters, you may need to switch between contexts. To change the active context to your `kind` cluster, run:

```bash
kubectl config use-context kind-eka-cluster
```

This sets the `eka-cluster` as the current active cluster.

---

### **Step 8: Deploy Applications or Test the Cluster**

Now that your Kubernetes multi-node cluster is up and running, you can start deploying applications, services, or running tests.

For example, to deploy a sample Nginx application on your cluster, run:

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort
```

This will create a deployment with Nginx and expose it on a NodePort so you can access it from outside the cluster.

To verify the deployment is running:

```bash
kubectl get pods
```

And to get the URL to access the Nginx service:

```bash
kubectl get services
```

---

### **Step 9: Cleanup (Optional)**

Once you are done with the cluster, you can delete it to free up resources:

```bash
kind delete cluster --name eka-cluster
```

This will delete the cluster, removing all the resources and nodes associated with it.
