##KUBERNETES ARCHITECTURE


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ux3fgoqnekv4ryiemmev.png)



Kubernetes clusters are composed of two main parts:
1. **Control Plane (Master Node)**
2. **Data Plane (Worker Nodes)**

### 1. **Control Plane**
The control plane is the brain of the Kubernetes cluster. It manages the cluster, deciding what runs where and how resources are allocated. The control plane consists of several key components:

- **API Server**: 
   - This is the central management point of the cluster. It exposes the Kubernetes API, which is used by all other components to interact with the cluster.
   - When a user wants to deploy an application, they send a request to the API Server. The API Server then processes this request and decides how to handle it.

- **Scheduler**: 
   - The scheduler is responsible for placing containers (which are wrapped in pods) onto the worker nodes. It decides which node is the best fit for a pod based on resource availability, policies, and other constraints.
   - For example, if a user requests to deploy a pod, the scheduler determines which node has enough CPU, memory, and other resources available to run it.

- **Controller Manager**: 
   - This component runs various controllers that are responsible for maintaining the desired state of the cluster. 
   - For example, the **ReplicaSet controller** ensures that the specified number of pod replicas are always running. If a pod fails, the controller manager notices this and schedules the creation of a new pod to maintain the desired number of replicas.

- **etcd**: 
   - `etcd` is a distributed key-value store that Kubernetes uses to store all cluster data, including the current state and configuration of the cluster. It is crucial for Kubernetes operations because it keeps the entire cluster's state in a consistent manner.
   - If `etcd` fails, the cluster could lose its state and configuration, so it's critical to back up this data.

- **Cloud Controller Manager**:
   - This component allows Kubernetes to interact with the underlying cloud provider (e.g., AWS, Azure, GCP). 
   - For instance, when a Kubernetes service needs to create a load balancer, the cloud controller manager translates the request into a format the cloud provider understands and then manages the cloud resources accordingly.
   - This component is especially important in hybrid or cloud-native Kubernetes environments.

### 2. **Data Plane (Worker Nodes)**
The worker nodes are where your application workloads actually run. Each worker node has several key components:

- **Kubelet**:
   - The `kubelet` is the primary agent that runs on each worker node. It communicates with the control plane and ensures that the containers in the pods are running as expected.
   - If a pod is not running, the `kubelet` will report back to the control plane, which may trigger a response to recreate or reschedule the pod.

- **Container Runtime**:
   - This is the software that actually runs containers on the node. Kubernetes can use different container runtimes, such as Docker, `containerd`, or `CRI-O`.
   - The container runtime is responsible for pulling container images, starting and stopping containers, and reporting container status back to the `kubelet`.

- **Kube-proxy**:
   - `Kube-proxy` is responsible for networking in Kubernetes. It maintains network rules on each worker node, allowing for communication between different pods within the cluster.
   - It also handles load balancing, ensuring that traffic is distributed evenly across the pods when you have multiple replicas of a service.

### Workflow

1. **Deploying a Pod**:
   - A user deploys a pod using a YAML file, which they submit to the API Server.
   - The API Server stores this request in `etcd` and forwards the request to the Scheduler.

2. **Scheduling the Pod**:
   - The Scheduler selects the best worker node for the pod based on the current resource availability and constraints.
   - Once a suitable node is selected, the Scheduler updates `etcd` with the new pod assignment.

3. **Running the Pod**:
   - The `kubelet` on the selected worker node notices the new pod assignment. It instructs the container runtime to pull the necessary container image and start the container inside a pod.
   - The `kubelet` then continuously monitors the pod’s status and reports back to the control plane.

4. **Networking and Load Balancing**:
   - If the pod is part of a service, `kube-proxy` ensures that network traffic is properly routed to this pod.
   - If the service has multiple replicas, `kube-proxy` balances the incoming traffic across all pods.

5. **Auto-healing and Scaling**:
   - If the pod crashes, the `kubelet` reports this to the control plane, which might trigger the Controller Manager to recreate the pod.
   - If auto-scaling is configured, the Controller Manager might increase or decrease the number of pod replicas based on load.
