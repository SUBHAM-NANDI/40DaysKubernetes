#### Step 1: Introduction to Kubernetes Services
Kubernetes services enable communication between various components of your cluster, such as between frontend and backend pods, or between pods and external users.

1. **What is a Service?**
   - A **Kubernetes service** is an abstraction that defines a logical set of pods and a policy to access them.
   - Services provide a way to expose applications running on a set of pods to users either inside the cluster (internal) or outside (external).

#### Step 2: Problem Definition
We had a **deployment** of four **nginx pods** serving a frontend application. However, the deployment wasn’t exposed to the external world. We can only access the pods internally within the cluster.

1. **Goal**: Make the application externally accessible using **Kubernetes services**.

#### Step 3: Types of Kubernetes Services
Kubernetes provides different service types to control how applications are exposed:

1. **ClusterIP**: Exposes the service on an internal IP of the cluster.
2. **NodePort**: Exposes the service on each node's IP at a static port.
3. **LoadBalancer**: Provisions a load balancer to expose the service externally.
4. **ExternalName**: Maps the service to an external DNS name.


#### Step 4: Exposing the Application Using NodePort

Let’s dive into the **NodePort** service type. Here, the service exposes the application on a static port in the range `30,000-32,767`, which is accessible externally.

1. **Scenario**: You have a frontend application running in a pod, and you want to expose it to the outside world using **NodePort**.
   - The frontend pod listens on **Port 80** inside the cluster.
   - The service exposes this application to users outside the cluster on **Port 30,001**.

2. **Key Ports in NodePort**:
   - **NodePort**: The port that is exposed externally, in this case, **30,001**.
   - **Port**: The port on which the service is available internally within the cluster, typically **Port 80**.
   - **TargetPort**: The port on which the application is listening, which is also **Port 80**.

#### Step 5: Creating a NodePort Service Manifest
We’ll now create a Kubernetes manifest file to define the **NodePort** service.

1. **Create the YAML file**:
   In your project directory, create a file called `nodeport.yaml`.
   
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: nodeport-svc
     labels:
       env: demo
   spec:
     type: NodePort
     selector:
       env: demo
     ports:
     - port: 80
       targetPort: 80
       nodePort: 30001
   ```

2. **Explanation**:
   - `apiVersion: v1`: The API version for the service.
   - `kind: Service`: This is a service definition.
   - `metadata`: Defines the service name (`nodeport-svc`) and labels.
   - `spec`: Contains the service specifications:
     - `type: NodePort`: This service will be of NodePort type.
     - `selector`: Matches the pods with the label `env: demo`.
     - `ports`: Specifies the ports:
       - `port`: Internal port on which the service will be available.
       - `targetPort`: The port where the application is running inside the pod.
       - `nodePort`: The external port where the service will be exposed.

#### Step 6: Deploy the Service
Now we need to deploy the service to our Kubernetes cluster.

1. **Apply the Service**:
   Run the following command in the terminal to create the service:
   
   ```bash
   kubectl apply -f nodeport.yaml
   ```

2. **Verify the Service**:
   After the service is created, check its status with:
   
   ```bash
   kubectl get svc
   ```
   
   You should see the `nodeport-svc` service with a **NodePort** assigned (e.g., `30,001`).

#### Step 7: Access the Application Externally
To access the application externally:

1. **Get the Node IP**:
   Run the following command to get the IP address of the node where the application is running:
   
   ```bash
   kubectl get nodes -o wide
   ```

2. **Access the Application**:
   Once you have the node’s IP, you can access the application using the Node IP and the NodePort, for example:
   
   ```
   http://<NODE-IP>:30001
   ```

#### Step 8: Special Case for Kind Cluster
If you’re using **Kind** (Kubernetes in Docker) for your Kubernetes cluster, you might face issues exposing the application externally due to how Kind handles port mapping.

1. **Why Kind Requires Extra Steps?**
   - Kind clusters are not directly exposed to your local machine, which is why NodePort services are not automatically accessible.
   
2. **Port Mapping for Kind Cluster**:
   You’ll need to modify the Kind cluster configuration to allow port mapping. This step involves updating the **Kind** YAML configuration to map the NodePort to your local machine.

   - Modify your `kind-config.yaml`:
   
     ```yaml
     kind: Cluster
     apiVersion: kind.x-k8s.io/v1alpha4
     nodes:
       - role: control-plane
         extraPortMappings:
           - containerPort: 30001
             hostPort: 30001
             protocol: TCP
     ```

3. **Recreate the Kind Cluster**:
   - Delete the existing cluster:
     ```bash
     kind delete cluster --name <your-cluster-name>
     ```
   - Create the new cluster with the updated configuration:
     ```bash
     kind create cluster --config kind-config.yaml --name <your-cluster-name>
     ```

4. **Access the Application**:
   After recreating the Kind cluster with port mapping, you can access your application locally using `localhost:30001`.

---

### Conclusion

In this video, we explored how to expose Kubernetes applications using **NodePort** services. We learned about the different types of services, understood the purpose of key ports like **NodePort**, **Port**, and **TargetPort**, and saw a hands-on example of creating and deploying a service. For Kind clusters, we covered the extra step of port mapping to make the service accessible externally.

