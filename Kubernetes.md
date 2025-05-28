# Kubernetes Interview Questions and Answers

## Q1: What is Kubernetes architecture?
Kubernetes has two main components:

1. Control plane (Master Node)
   This is the brain of the kubernetes cluster, it manages the state of cluster.
   Components in control plane:
   1. API server:
       Entry point for all commands(via kubectl). It validates and process REST requests
       It exposes k8s to the external world, it takes all the incoming requests
   2. Etcd
       The cluster key-value store, it holds the state and configuration of cluster, in other words backing all cluster data
   3. Kube scheduler 
       Assigns newly created pods to appropriate nodes based on the resources, availability, affinity rules.
   4. Control Manager
       It is responsible for running the controllers, k8s has many controllers like node controller, replica controller. 

2. Data plane (Worker Node)
   1. Kubelet
       Talks to API, and responsible for ensuring the pods are running. if pod isn't running it notify API server.
   2. Kube-proxy
       Maintain networking rules and forward traffic to the correct pods.
       It provides Networking, IP addresses, and load balancing
   3. Container runtime
       You need a runtime to run containers or docker, its responsible for running the actual containers.

## Q2: What is the difference between Deployment and StatefulSet?
In kubernetes, both deployment and stateful sets are used to manage pods, but they serve different purpose based on whether the application is stateless or stateful.

A deployment is used for stateless application. It manages replica sets and ensures that any pod can be replaced with another as they are identical.
Ideal for web servers, microservices, REST APIs.

On the other hand, Stateful sets are designed for stateful applications that need stable identity, persistent storage and stable DNS, and order deployment. 
Ex: databases like MongoDB, Kafka

## Q3: How to run a yaml manifest without a yaml manifest file created?
To run a Kubernetes YAML manifest without creating a file, I use kubectl apply -f - and pass the YAML content directly through stdin

## Q4: If there is file which is being used by 2 customers, and need to deploy that file in k8s cluster and on prem as well, how to do that?
If I need to deploy the same file to k8s and on-prem, I will store the file in a central location like git repo, nexus, S3 etc.
For Kubernetes, I use a ConfigMap or volume mount in the deployment spec, and manage deployments using Helm or GitOps.
For on-prem, I use Ansible, SCP, or custom scripts to copy the file and restart the app if needed.

## Q5: How to deploy an app to k8s cluster in terms of app deploy only (basically explain CD part)?
Deploying an app to cluster is depends on how you set your CD:
1. You need to update the new version of image in yaml file
2. Apply to the Kubernetes Cluster using `kubectl apply`

I use GitOps approach to achieve CD using tool called ArgoCd.

## Q6: If a secret is stored in a Vault (like HashiCorp Vault) and the Pod consuming that secret is down, you'll need to perform TSG (Troubleshooting Guide)?
1. Check Pod Status and Logs
2. Check Vault Access Method
   Depending on how your app accesses Vault secrets, your TSG will vary.
3. Check Vault Itself
   Ensure the Vault server is:
   ✅ Running
   ✅ Unsealed
   ✅ Accessible from the pod's namespace
4. Restart Pod or Trigger Re-deploy

## Q7: In Kubernetes, if a pod is in a pending state, how do you troubleshoot?
If pod is in a pending state, it means its not assigning to any node. Firstly, I do `kubectl describe pod <pod-name> -n <namespace>` to check the reason why pod is not scheduling to any node. 
I also check pod resource request like does it need more resources, check node conditions, or any affinity rules, node selector.

## Q8: What will be your approach if pod.yaml failed?
1. Check the Immediate Error
   I first look at the error returned by `kubectl apply -f pod.yaml` — it usually points to YAML syntax issues, missing fields, or schema validation errors.
2. Validate the YAML
   ```bash
   kubectl apply --dry-run=client -f pod.yaml
   ```
3. Inspect the Pod Events
   ```bash
   kubectl describe pod <pod-name>
   ```
4. View Container Logs
   ```bash
   kubectl logs <pod-name>
   ```
Based on the issue, I take action.

## Q9: There are 1 Master & 3 Worker nodes - if the master fails, what happens? Will pods keep running or they will crash?
If master node fails, the existing pods will keep running, they do not crash immediately but cluster management stops that being said, no new pods, no scaling, no healing. 

But the existing workloads continue running because the worker nodes and Kubelets are still alive.

## Q10: In K8s, as etcd is a key-value store db, can write something manually on it?
- You can technically write to etcd manually, it is not recommended
- Use Kubernetes APIs (kubectl, controllers, CRDs, etc.) to update the cluster state. The API server handles validation and then persists the data to etcd safely.

The only time I've accessed etcd directly was during disaster recovery or low-level debugging, such as:
- Backing up etcd
- Restoring the cluster from a snapshot
- Checking orphaned or corrupted entries (read-only)

## Q11: How to roll back a failed deployment in Docker & K8s?
For Docker, we switched the image tag back and redeployed using Docker Compose.
Kubernetes has built-in support for rollback in Deployments.
```bash
kubectl rollout undo deployment <deployment-name>
```
We used `kubectl rollout undo` to instantly roll back to the previous stable version while logs were investigated.

## Q12: What is the difference between an EBS-backed instance and a non-EBS-backed instance?
EBS-Backed Instance:
- The root volume is stored on Amazon EBS (Elastic Block Store) — which is a network-attached, persistent storage.
- You can stop, start, reboot, and snapshot the instance without losing data.
- The root volume can persist even after termination (if DeleteOnTermination=false).
- You can resize the root volume or move it to another instance.
✅ Used for: Long-running workloads, persistent applications, and systems requiring backup/recovery.

Non-EBS (Instance Store)–Backed Instance:
- The root volume is stored on ephemeral instance store, which is physically attached to the host machine.
- Data is lost when the instance is stopped, terminated, or crashes.
- These instances cannot be stopped, only rebooted or terminated.
- Snapshotting is not supported for instance store root volumes.
✅ Used for: Temporary workloads, buffers, caches, or stateless processing.

## Q13: I have 3 nodes (small, medium, and large), and I want only data load to go to the large node. How can I do that?
To ensure that data load pods go only to the large node, First label the node and I would use Kubernetes scheduling rules like node selectors, node affinity, or taints and tolerations. These mechanisms control where pods are scheduled based on node labels and characteristics.

## Q14: I am getting the following error, how can I debug that and what does the error mean?
Pods fail to schedule
0/5 nodes are available: insufficient memory.

The error 0/5 nodes are available: insufficient memory means the pod's memory request cannot be satisfied by any node.
I would:
- Inspect pod memory requests
- Check node memory capacity and usage
- Adjust requests or scale the cluster accordingly

## Q15: What is the difference between scaling and autoscaling in Kubernetes?
1. Scaling (Manual Scaling)
   Scaling is when you manually increase or decrease the number of pod replicas or nodes based on your own analysis or requirement.
2. Autoscaling (Dynamic Scaling)
   Autoscaling is automatic — Kubernetes adjusts resources based on real-time metrics like CPU, memory, or custom metrics.

## Q16: If I don't specify TargetPort in the service object, what is it going to do?
If you don't specify targetPort in a Kubernetes Service object, Kubernetes will default the targetPort to the same value as the port field.

## Q17: What are the different types of secrets in Kubernetes?
In my experience, I mostly work with:
1. Opaque (default)
   This is the most commonly used type. It stores generic key-value pairs such as:
   - API keys
   - DB passwords
   - Tokens

2. kubernetes.io/tls
   Used for storing TLS certificates and private keys in PEM format. It's typically used for Ingress TLS termination or mutual TLS between services.

3. kubernetes.io/dockerconfigjson
   Stores Docker registry credentials to pull images from private container registries.

## Q18: I have an Ingress object that is not routing the traffic to the Kubernetes cluster. What are the reasons and how do you troubleshoot that?
1. Check if the Ingress Controller is Installed and Running
2. Check the Ingress Resource Syntax and Annotations
3. Verify DNS Resolution and External IP
4. Check Backend Service and Endpoints
5. Check Target Pod Readiness Probes
6. Check TLS Configuration (if HTTPS is used)
7. Check Logs of the Ingress Controller

## Q19: I have created a service object that is not mapped to a deployment. What could be the reason and how do you debug it?
If a Service isn't mapping to a Deployment:
- Check label-selector mismatch
- Inspect endpoints
- Verify ports and namespaces
- Ensure pods are running and ready

## Q20: What are the different ways to specify the probes in Kubernetes?
In Kubernetes, probes are used to monitor the health of containers and ensure traffic is only sent to healthy pods. There are three types of probes, and each can be configured in different ways to check the application status.

Probe Type | Purpose
---|---
Liveness Probe | Checks if the container is alive; restarts it if it fails
Readiness Probe | Checks if the container is ready to serve traffic
Startup Probe | Checks if the container is still starting up; useful for slow-starting apps

Kubernetes supports Liveness, Readiness, and Startup probes. Each can be implemented using:
- HTTP GET — for web health endpoints
- TCP Socket — for checking if a port is open
- Exec Command — for custom shell checks

## Q21: If I select the restart policy as Never, what is it going to do?
If the restart policy is set to Never, Kubernetes will not restart the container in the pod after it exits, regardless of whether it exited successfully or with an error.

## Q22: What is an init container and why do we need to use it?
An Init Container in Kubernetes is a special type of container that runs before the main application containers in a Pod. It is used to prepare or set up the environment for the main container and must complete successfully before the main containers start.
They ensure that the environment is ready, dependencies are met, and responsibilities are cleanly separated — making pods more reliable and modular.

## Q23: How to use the secrets in kubernetes? What encryption methods do you use?
In Kubernetes, I use Secrets to securely store sensitive data such as API keys, passwords, tokens, TLS certs. I integrate them into workloads via environment variables, volumes, or reference in other resources. 
While ensuring they're encrypted at rest in etcd using AES or KMS, and access-controlled via RBAC. For advanced scenarios, I integrate with external secret managers for rotation and dynamic access.

## Q24: What will be the command to add the annotation and the labels for the existing pod?
You can use the kubectl annotate and kubectl label commands to add or modify annotations and labels on an existing pod.
Annotations are used for metadata consumed by tools/controllers (e.g., sidecar.istio.io/inject=false)
Labels are used for selection and grouping (e.g., in Services, Deployments, etc.)

## Q25: What is Taint/Toleration?
In Kubernetes, Taints and Tolerations work together to control which pods can be scheduled on which nodes.

What is a Taint?
A taint is a property applied to a node that tells the Kubernetes scheduler:
"Do NOT schedule pods here unless they tolerate this taint."

What is a Toleration?
A toleration is a property added to a pod that tells Kubernetes:
"I'm allowed to run on nodes with this specific taint."

## Q26: What are the use cases of Node-Port and Cluster IP service Type in Kubernetes?
In Kubernetes, ClusterIP and NodePort are two common Service types used to expose applications, and they serve different purposes depending on the scope of accessibility.

1. ClusterIP:
   - ClusterIP is default service type which exposes service only inside the kubernetes cluster
   ✅ When to Use:
   - Microservices talking to each other (e.g., frontend → backend)
   - Internal-only databases, APIs, or message queues
   - Backends accessed via Ingress or LoadBalancer

2. NodePort
   🔧 Use Case: Expose Application on Static Port on Every Node
   - NodePort exposes the service on the same port (e.g., 30080) on every node in the cluster.
   When to Use:
   - For development, testing, or POC environments where you don't need a load balancer

## Q27: What is PDB in Kubernetes?
PDB stands for Pod Distribution Budget in Kubernetes.

It is used to control the number of pods that can be voluntarily disrupted at any given time — helping ensure high availability during planned operations like node maintenance, cluster upgrades, or scaling events.

Why Use PDB?
- Without a PDB, Kubernetes might evict all pods of a Deployment or StatefulSet during a rolling update or node drain.
- A PDB protects against too many pods being unavailable at once.

How It Works
You define a PDB with either:
- minAvailable: Minimum number of pods that must remain available

## Q28: What is the difference between PV/PVC in Kubernetes?
In Kubernetes, PV (PersistentVolume) and PVC (PersistentVolumeClaim) work together to provide persistent storage for pods:

1. PV – PersistentVolume
   - A PV is a pre-provisioned piece of storage in the cluster.
   - It represents a physical volume (like an EBS volume, NFS share, or CSI-backed disk) that is managed by the admin or dynamically provisioned.

2. PVC – PersistentVolumeClaim
   - A PVC is a request for storage made by a user or a pod.
   - The PVC defines how much storage the pod needs, and what access mode it requires.

There are two ways:
- Static provisioning: Admin manually creates a PersistentVolume object.
- Dynamic provisioning: Kubernetes automatically provisions a PV using a StorageClass when a PVC is created.

PV is storage in the cluster, developer creates a PVC which defines how storage, access mode. So kubernetes matches the PVC to a suitable PV then Pod uses the PVC to mount the volume. Finally data is persisted independently of pod, If the pod is deleted or restarted, the data remains intact on the PV.

## Q29: How can you use an existing Image in a YAML file to deploy a POD?
To use an existing image in a YAML file, define a pod with `image: <image-name>` in the container spec.
Kubernetes pulls the image and runs the container inside the pod — enabling fast deployment using prebuilt Docker images.

## Q30: What is POD in Kubernetes?
A POD is the smallest deployable unit in the cluster where you run the container.

## Q31: What are the Types of Service in Kubernetes?
1. ClusterIP
2. NodePort
3. LoadBalancer

## Q32: What are Namespaces in Kubernetes?
In Kubernetes, a Namespace is a logical partition within a cluster that allows you to organize, isolate, and manage resources like pods, services, and deployments. It's especially useful in multi-tenant environments or when you want to separate environments (e.g., dev, test, prod) within the same cluster.

## Q33: What is HPA (Horizontal Pod Autoscaler)?
HPA is a Kubernetes resource that automatically scales the number of pod replicas in a Deployment, StatefulSet, or ReplicaSet based on observed CPU utilization or custom metrics.

## Q34: How to roll back to a specific version in Kubernetes?
```bash
kubectl rollout undo deployment <deployment-name> --to-revision=<number>
```

## Q35: What is Calico?
Calico is a CNI plugin and network security engine for Kubernetes that provides high-performance networking, fine-grained network policy enforcement, and scalability across cloud and on-prem environments. It's ideal for teams looking to implement secure, scalable, and observable network infrastructure in Kubernetes.

## Q36: How to take backup of Kubernetes cluster?
There are number of ways to backup the kubernetes cluster:

1. Backup Cluster Resources (YAML Manifests)
   ✅ Manual Backup Using kubectl

2. Backup Persistent Volumes
   Use storage-level snapshots (EBS, Azure Disk, GCE PD)

3. Full Cluster Backup with Velero (Recommended)
   Velero is a popular open-source tool for backup and recovery of Kubernetes cluster state and volumes.

4. Etcd Backup (For Control Plane/Stateful Clusters)
   If you manage a self-hosted control plane, backing up etcd (Kubernetes' internal key-value store) is crucial.

Since I am using AWS EKS the Kubernetes control plane is fully managed by AWS, so you don't need to back up etcd or the control plane manually.
Instead, you focus on backing up:
- Cluster configuration (manifests, policies, etc.)
- Application data (persistent volumes)

The most reliable and AWS-integrated way to back up EKS is using Velero with AWS S3 and EBS snapshots:

1. Backup Kubernetes Resources with Velero
   Velero can back up:
   - Deployments, Services, ConfigMaps, Secrets, etc.
   - Persistent volumes via AWS EBS volume snapshots

✅ Install Velero:
2. Create a Backup
```bash
velero backup create eks-daily-backup --include-namespaces=default,prod
```
3. Schedule Automatic Backups

On EKS:
- Control plane is managed by AWS — no etcd backup needed
- Use Velero to back up workloads + persistent volumes to S3
- Use IAM roles (IRSA) for secure access
- Optionally schedule backups and restore via Velero CLI or CRDs

## Q37: How to upgrade EKS cluster?
In Amazon EKS, I upgrade the Kubernetes cluster in two stages: first the control plane, then the data plane (nodes or Fargate). I ensure zero downtime by using managed rolling upgrades, readiness probes, and PodDisruptionBudgets to keep services highly available throughout.

1. Upgrade the Control Plane (Managed by AWS)
   `eksctl upgrade cluster --name <cluster-name>`
   ✅ AWS handles the upgrade with no impact on running workloads
   ✅ Compatible API versions must be ensured first

2. Upgrade Node Groups (Managed Node Groups)
   Upgrade worker nodes after the control plane to maintain compatibility.

   Option 1: Rolling upgrade using eksctl
   `eksctl upgrade nodegroup --cluster <name> --name <nodegroup-name> --kubernetes-version <version> --approve`
   - AWS replaces old nodes one by one
   - Uses cordon + drain + replace pattern

   Option 2: Replace with a new node group
   - Create a new MNG with updated version/AMI 
   - Migrate workloads using labels or taints
   - Delete old node group when empty
   ✅ Safer for large or sensitive workloads

- Fargate profiles are automatically upgraded behind the scenes by AWS

## Q38: Can we run 1 container with 2 pods?
No, you can't run 1 container in 2 pods.

## Q39: What is Ingress Controller?
An Ingress Controller is a Kubernetes component that manages external HTTP/HTTPS access to services running inside a cluster. It watches Ingress resources and routes external traffic to the appropriate backend services based on rules like hostnames, paths, or headers.

## Q40: If you have taken etcd backup and old VM corrupted, can we create new VM with backup etcd?
Yes, you can create a new VM and restore your Kubernetes cluster using the etcd snapshot you previously took.
This is a standard disaster recovery strategy for clusters managed via kubeadm or self-hosted control planes.

## Q41: In a StatefulSet with 3 pods (mongo-0, mongo-1, mongo-2), what happens if mongo-0 dies? What will be the new pod name?
In a StatefulSet, the pod names are stable and unique, based on the StatefulSet name and an ordinal index. So if mongo-0 dies, Kubernetes will recreate it with the same name — that is, it will come back as mongo-0, not a new name.

## Q42: How does rolling updates strategy work for Deployments and StatefulSets?
In Kubernetes, rolling updates are handled differently for Deployments and StatefulSets, because of the nature of pod identity and ordering.

1. Rolling Update Strategy for a Deployment
   Deployments manage stateless applications and prioritize speed and availability.
   - Pods are updated in parallel or batches
   - No guaranteed order
   - Can update multiple pods at once based on:
     - maxUnavailable
     - maxSurge
   ```bash
   strategy:
     type: RollingUpdate
     rollingUpdate:
       maxUnavailable: 1
       maxSurge: 1
   ```

2. Rolling Update Strategy for a StatefulSet
   StatefulSets manage stateful applications like databases (e.g., MongoDB, Kafka), so updates are more conservative.
   - Pods are updated one at a time in reverse ordinal order
   - Pod pod-N is terminated and updated only after pod-(N+1) is Running and Ready
   - Ensures data safety and service continuity
   ```bash
   updateStrategy:
     type: RollingUpdate
   ```

## Q43: Can we deploy services on master node?
Yes! technically we can deploy but not at all recommended and I did not deploy any services on master node.

## Q44: How to assign memory to pod and handle memory constraints?
In Kubernetes, you assign memory to a pod by setting requests and limits for memory in the pod or container spec.

```bash
resources:
  requests:            //Minimum amount of memory the pod needs. Used by the scheduler.
      memory: "256Mi"
  limits:              // Maximum amount of memory the pod can use. If exceeded, it gets OOMKilled.
      memory: "512Mi"
```

## Q45: What is the difference between Role and RoleBinding?
In Kubernetes, Role and RoleBinding are used together to control who can do what within a namespace (RBAC – Role-Based Access Control).

1. Role – Defines Permissions
   A Role is a Kubernetes object that defines a set of allowed actions (like get, list, create, etc.) on specific resources (like pods, secrets, services) within a namespace.

2. RoleBinding – Grants Access
   A RoleBinding binds a Role to a user, group, or service account, effectively granting the permissions defined in the Role.

## Q46: What to do when pods are not removed during node drain?
1. Identify Why Pods Are Stuck
2. Check Pod Disruption Budget (PDB)
   ```bash
   kubectl get pdb -A
   ```
   If PDB is blocking eviction (e.g., minAvailable=1 and only 1 pod running), either:
   - Scale the app temporarily
   - Modify or delete the PDB
3. Force Delete the Pod (Last Resort)
   Only do this after confirming the pod can be safely removed (e.g., it's stateless)
4. Use kubectl cordon + Monitor Manually
   If drain fails, cordon the node to prevent new pods from scheduling and manually move/scale pods off it.
   ```bash
   kubectl cordon <node-name>
   ```

## Q47: What's the purpose of using StorageClass in K8s?
A StorageClass in Kubernetes defines the "class" or type of storage you want to use for dynamic provisioning of PersistentVolumes (PVs).

1. Enable Dynamic Volume Provisioning
   Without a StorageClass, you have to manually create PVs before claims (PVCs) can use them.
   ✅ With a StorageClass:
   - A PersistentVolume is automatically created when a user creates a PersistentVolumeClaim (PVC)
   - It saves time and avoids manual intervention

In our Kubernetes cluster, we used multiple StorageClasses:
- gp3 for general apps
- io2 for databases
- efs-sc for shared access with ReadWriteMany

Each PVC specified its desired class, and volumes were automatically provisioned based on performance and availability needs.

## Q48: Can two PODs in different worker nodes communicate?
Yes, by default in Kubernetes, any pod can communicate with any other pod, regardless of whether they are running on the same or different worker nodes, as long as they are:
- In the same cluster
- Not restricted by Network Policies

This is because Kubernetes uses a flat, routable network with pod-to-pod communication enabled across nodes by the CNI plugin (like Calico, Cilium, Flannel).

## Q49: How to restrict communication between pods?
To restrict that, use NetworkPolicies with supported CNI plugins:
- Start with a deny-all policy
- Then explicitly allow required traffic

## Q50: What components should be added in NetworkPolicy YAML file?
The key components in a NetworkPolicy YAML are:
- podSelector – selects target pods
- policyTypes – defines traffic direction (Ingress/Egress)
- ingress/egress – rules that allow specific sources/destinations and ports

These components work together to control network access to and from pods.

## Q51: What is deployment.yml?
A deployment.yaml file in Kubernetes is a YAML manifest used to define a Deployment resource.
A Deployment manages the declarative deployment, scaling, and updates of a group of replicated pods. It's one of the most common and powerful Kubernetes objects.

What Does deployment.yaml Contain?
It includes the desired configuration for:
- Number of replicas
- Pod template (containers, images, ports, env vars)
- Rolling update strategy
- Labels & selectors
- Optional: Probes, resource limits, volumes, etc.

## Q52: What is service.yml?
A service.yaml file in Kubernetes is a YAML manifest that defines a Service resource, which provides a stable network endpoint to expose a set of pods.
It enables internal or external access to pods, load balances traffic, and abstracts dynamic pod IPs.

📦 Why Do We Need a Service?
- Pods in Kubernetes are ephemeral (can restart or move across nodes)
- A Service provides a permanent DNS name and IP to reach those pods
- It load balances traffic to all matching pods using label selectors

## Q53: What is ReplicaSet?
A ReplicaSet in Kubernetes ensures that a specified number of identical pod replicas are running at all times.
It automatically creates new pods if some fail or get deleted, and removes extra pods if there are too many.

## Q54: What is the output of Helm chart?
The output of a Helm chart is a set of rendered Kubernetes resource manifests (YAML files) based on templates and user values.
Helm simplifies deployment by packaging infrastructure and automating configuration management.

## Q55: What files are available inside Helm chart?
A Helm chart is a structured directory that contains a set of files and folders used to define, configure, and deploy Kubernetes applications.
These files help Helm template and render Kubernetes manifests dynamically based on user input.

```bash
mychart/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── _helpers.tpl
│   └── ingress.yaml
├── charts/
├── README.md (optional)
└── .helmignore
```

## Q56: What is CoreDNS in K8s?
CoreDNS is the default DNS server for Kubernetes clusters.
It provides name resolution for services, pods, and external domains within the cluster, allowing applications to communicate with each other using DNS names instead of IP addresses.

Purpose of CoreDNS in Kubernetes:
- Enables service discovery by translating service names to cluster IPs
- Resolves pod DNS names in the format:
  pod-name.namespace.svc.cluster.local
- Handles external DNS resolution (e.g., google.com)
- Supports custom DNS plugins and configuration

## Q57: What's the purpose of CNI?
CNI (Container Network Interface) is a specification and plugin system used by Kubernetes to manage container networking.
The purpose of CNI is to ensure that each pod gets a network interface, IP address, and routing rules so that pods can communicate with each other and with the external world.

How CNI Works:
1. Kubernetes tells the kubelet to create a pod.
2. Kubelet calls the CNI plugin (e.g., Calico, Flannel, Cilium) to:
   - Attach a network interface to the pod
   - Assign an IP address
   - Configure routing and firewall rules

## Q58: How does kube-proxy communicate with nodes?
kube-proxy runs on every node in a Kubernetes cluster and is responsible for maintaining network rules to route traffic to the correct pod IPs or services.
It doesn't need to talk to other nodes directly — instead, it communicates with the Kubernetes API Server and programs routing rules on its local node.

## Q59: What is the difference between ClusterRole and ClusterRoleBinding?
In Kubernetes, ClusterRole and ClusterRoleBinding are similar to Role and RoleBinding but with cluster-wide scope:

1. ClusterRole
   - Defines permissions that can be used across the entire cluster
   - Not limited to a specific namespace
   - Can be used to grant access to cluster-level resources (nodes, PVs) or resources in all namespaces

2. ClusterRoleBinding
   - Binds a ClusterRole to users, groups, or service accounts
   - Grants permissions across the entire cluster
   - Used for cluster administrators or components that need cluster-wide access

## Q60: How will you know if a NetworkPolicy is enabled or not in K8s?
To check if NetworkPolicy is enabled in your Kubernetes cluster:

1. Check if your CNI plugin supports NetworkPolicy:
   ```bash
   kubectl get pods -n kube-system
   ```
   Look for CNI plugins like Calico, Cilium, or Weave Net

2. Verify NetworkPolicy API is available:
   ```bash
   kubectl api-resources | grep networkpolicies
   ```

3. Test with a simple NetworkPolicy:
   ```bash
   kubectl apply -f - <<EOF
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
     name: test-network-policy
   spec:
     podSelector: {}
     policyTypes:
     - Ingress
     - Egress
   EOF
   ```

## Q61: What key things should you verify post-upgrade?
After upgrading a Kubernetes cluster, verify:

1. Cluster Health
   ```bash
   kubectl get nodes
   kubectl get pods -A
   ```

2. Core Components
   - API Server is accessible
   - CoreDNS is running
   - CNI plugin is functioning
   - Storage classes are available

3. Applications
   - All deployments are running
   - Services are accessible
   - Ingress controllers are working
   - Persistent volumes are mounted

4. Monitoring and Logging
   - Metrics collection is working
   - Log aggregation is functioning
   - Alerts are properly configured

## Q62: How to set up Jenkins pipeline for Kubernetes?
To set up a Jenkins pipeline for Kubernetes:

1. Install Required Plugins
   - Kubernetes Plugin
   - Docker Pipeline
   - Git Integration

2. Configure Kubernetes Credentials
   ```groovy
   pipeline {
     agent {
       kubernetes {
         yaml """
           apiVersion: v1
           kind: Pod
           spec:
             containers:
             - name: docker
               image: docker:latest
               command:
               - cat
               tty: true
         """
       }
     }
     stages {
       stage('Build') {
         steps {
           container('docker') {
             sh 'docker build -t myapp:${BUILD_NUMBER} .'
           }
         }
       }
       stage('Deploy') {
         steps {
           sh 'kubectl apply -f k8s/'
         }
       }
     }
   }
   ```

## Q63: If a pod is getting restarted constantly, what steps are you going to follow?
When a pod is constantly restarting:

1. Check Pod Status
   ```bash
   kubectl get pods
   kubectl describe pod <pod-name>
   ```

2. Check Container Logs
   ```bash
   kubectl logs <pod-name>
   kubectl logs <pod-name> --previous
   ```

3. Check Events
   ```bash
   kubectl get events --sort-by='.lastTimestamp'
   ```

4. Common Issues to Investigate:
   - Resource constraints (OOMKilled)
   - Liveness probe failures
   - Application crashes
   - Configuration issues
   - Network problems

## Q64: For deployment if you get timeout issue, what kind of API gateway you used?
For handling timeout issues in deployments, I've used:

1. NGINX Ingress Controller
   - Configurable timeouts
   - Load balancing
   - SSL termination

2. Kong API Gateway
   - Advanced rate limiting
   - Circuit breaking
   - Request/response transformation

3. AWS API Gateway
   - Integration with EKS
   - Built-in monitoring
   - Automatic scaling

## Q65: How to configure Cluster Auto-scaling?
To configure Cluster Auto-scaling in Kubernetes:

1. For AWS EKS:
   ```bash
   eksctl create cluster \
     --name my-cluster \
     --region us-west-2 \
     --nodegroup-name standard-workers \
     --node-type t3.medium \
     --nodes 3 \
     --nodes-min 1 \
     --nodes-max 4 \
     --managed
   ```

2. Install Cluster Autoscaler:
   ```bash
   helm repo add autoscaler https://kubernetes.github.io/autoscaler
   helm install cluster-autoscaler autoscaler/cluster-autoscaler \
     --set autoDiscovery.clusterName=my-cluster
   ```

3. Configure Auto-scaling Parameters:
   - Scale up/down thresholds
   - Node group configurations
   - Resource utilization targets

## Q66: How will you create HPA (Horizontal Pod Autoscaler)?
To create a Horizontal Pod Autoscaler:

1. Basic HPA with CPU metrics:
   ```yaml
   apiVersion: autoscaling/v2
   kind: HorizontalPodAutoscaler
   metadata:
     name: my-app-hpa
   spec:
     scaleTargetRef:
       apiVersion: apps/v1
       kind: Deployment
       name: my-app
     minReplicas: 1
     maxReplicas: 10
     metrics:
     - type: Resource
       resource:
         name: cpu
         target:
           type: Utilization
           averageUtilization: 80
   ```

2. Apply the HPA:
   ```bash
   kubectl apply -f hpa.yaml
   ```

3. Monitor HPA:
   ```bash
   kubectl get hpa
   kubectl describe hpa my-app-hpa
   ```

## Q67: If there are two clusters running in different regions, if there is an issue with one cluster, how to shift the traffic to another cluster?
To shift traffic between clusters in different regions:

1. Use Global Load Balancer
   - AWS Route 53 with health checks
   - Google Cloud Global Load Balancing
   - Azure Traffic Manager

2. Implement Health Checks
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     type: LoadBalancer
     ports:
     - port: 80
     selector:
       app: my-app
   ```

3. Configure DNS Failover
   - Set up health checks
   - Configure failover routing
   - Set appropriate TTL values

## Q68: If you type kubectl get pods, what will happen in the backend?
When you run `kubectl get pods`, the following happens:

1. kubectl sends a GET request to the Kubernetes API server
2. API server authenticates and authorizes the request
3. API server retrieves pod information from etcd
4. API server processes and filters the data
5. Response is sent back to kubectl
6. kubectl formats and displays the output

## Q69: How to check the kubectl logs of a POD before it restarted?
To check logs of a pod before it restarted:

```bash
kubectl logs <pod-name> --previous
```

This shows the logs from the previous instance of the container in the pod.

## Q70: Have you deployed any security applications on Kubernetes?
Yes, I've deployed several security applications:

1. Falco
   - Runtime security monitoring
   - Threat detection
   - Behavioral analysis

2. OPA (Open Policy Agent)
   - Policy enforcement
   - Access control
   - Configuration validation

3. Vault
   - Secret management
   - Dynamic credentials
   - Encryption as a service

## Q71: What service to use for sticky session as an alternative option?
For sticky sessions in Kubernetes:

1. Use Session Affinity in Services:
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     sessionAffinity: ClientIP
     sessionAffinityConfig:
       clientIP:
         timeoutSeconds: 10800
   ```

2. Use Redis for Session Storage:
   - Deploy Redis as a StatefulSet
   - Configure applications to use Redis for session storage
   - Provides persistence and scalability

## Q72: During a sticky session problem, what will be the cause for Load Balancer?
Common causes of sticky session problems with Load Balancers:

1. Load Balancer Configuration
   - Incorrect session affinity settings
   - Cookie configuration issues
   - Timeout settings

2. Application Issues
   - Session not properly stored
   - Cookie handling problems
   - Session replication issues

3. Network Problems
   - Load balancer health check failures
   - Network latency
   - Connection timeouts

## Q73: Do you create clusters in multi-regions? Is it possible? If yes, then how will you manage them?
Yes, it's possible to create and manage multi-region clusters:

1. Cluster Setup
   - Deploy clusters in different regions
   - Use managed services (EKS, GKE, AKS)
   - Configure networking between regions

2. Management Tools
   - Use GitOps tools (ArgoCD, Flux)
   - Implement centralized monitoring
   - Set up cross-region backups

3. Traffic Management
   - Use global load balancers
   - Implement health checks
   - Configure failover routing

4. Data Management
   - Set up cross-region replication
   - Implement backup strategies
   - Configure disaster recovery

## Q74: What is the difference between DaemonSet and StatefulSet?
In Kubernetes, DaemonSet and StatefulSet serve different purposes:

1. DaemonSet
   - Ensures all (or some) nodes run a copy of a Pod
   - As nodes are added to the cluster, pods are added to them
   - As nodes are removed, pods are garbage collected
   - Used for cluster-wide services like:
     - Logging (Fluentd)
     - Monitoring (Node Exporter)
     - Storage (GlusterFS)

2. StatefulSet
   - Manages stateful applications
   - Provides stable, unique network identifiers
   - Stable, persistent storage
   - Ordered, graceful deployment and scaling
   - Used for applications like:
     - Databases (MongoDB, MySQL)
     - Message queues (Kafka)
     - Cache systems (Redis)

## Q75: What is the difference between subnet and NACL?
In AWS networking:

1. Subnet
   - A logical division of an IP network
   - Can be public or private
   - Defines IP address range
   - Can span multiple Availability Zones
   - Used to organize and secure network resources

2. Network ACL (NACL)
   - A stateless firewall at the subnet level
   - Controls traffic in and out of subnets
   - Rules are evaluated in order
   - Can allow or deny specific IP addresses
   - Can be associated with multiple subnets

## Q76: What is the difference between NAT Gateway and Internet Gateway?
In AWS networking:

1. Internet Gateway
   - Allows communication between VPC and the internet
   - Provides a target in route tables for internet-routable traffic
   - Performs NAT for instances with public IPs
   - One per VPC
   - Used for public subnets

2. NAT Gateway
   - Allows instances in private subnets to connect to the internet
   - Prevents the internet from initiating connections to private instances
   - Managed by AWS
   - Highly available within a single Availability Zone
   - Used for private subnets

## Q77: How would you trigger Pipeline B in Jenkins automatically after Pipeline A?
There are several ways to trigger Pipeline B after Pipeline A in Jenkins:

1. Using Pipeline Triggers
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Pipeline A') {
         steps {
           // Pipeline A steps
         }
       }
     }
     post {
       success {
         build job: 'Pipeline-B'
       }
     }
   }
   ```

2. Using Jenkins API
   ```groovy
   pipeline {
     agent any
     stages {
       stage('Pipeline A') {
         steps {
           // Pipeline A steps
           sh 'curl -X POST http://jenkins:8080/job/Pipeline-B/build'
         }
       }
     }
   }
   ```

3. Using Jenkins Plugins
   - Parameterized Trigger Plugin
   - Build Pipeline Plugin
   - Job DSL Plugin

## Q78: What will happen when an IaC managed resource is modified manually?
When an Infrastructure as Code (IaC) managed resource is modified manually:

1. State Drift
   - The actual state differs from the desired state in code
   - Can cause conflicts in future deployments
   - May lead to unexpected behavior

2. How to Avoid
   - Use state locking
   - Implement proper access controls
   - Use automated monitoring
   - Regular state validation
   - Document all manual changes

## Q79: How would you set up networking in VPC?
To set up networking in an AWS VPC:

1. Create VPC and Subnets
   ```hcl
   resource "aws_vpc" "main" {
     cidr_block = "10.0.0.0/16"
     enable_dns_support = true
     enable_dns_hostnames = true
   }

   resource "aws_subnet" "public" {
     vpc_id = aws_vpc.main.id
     cidr_block = "10.0.1.0/24"
     availability_zone = "us-west-2a"
   }

   resource "aws_subnet" "private" {
     vpc_id = aws_vpc.main.id
     cidr_block = "10.0.2.0/24"
     availability_zone = "us-west-2a"
   }
   ```

2. Configure Route Tables
   - Public route table with Internet Gateway
   - Private route table with NAT Gateway

3. Set up Security Groups and NACLs
   - Define inbound/outbound rules
   - Configure subnet-level access

4. Configure DNS and DHCP options

## Q80: How will you direct traffic to and from an instance in a private subnet?
To direct traffic to and from an instance in a private subnet:

1. Outbound Traffic
   - Use NAT Gateway or NAT Instance
   - Configure route table to route internet traffic through NAT
   - Ensure security groups allow required outbound traffic

2. Inbound Traffic
   - Use Application Load Balancer (ALB)
   - Configure target groups
   - Set up health checks
   - Use security groups to control access

3. Internal Traffic
   - Use VPC endpoints for AWS services
   - Configure security groups for internal communication
   - Use private DNS for service discovery
