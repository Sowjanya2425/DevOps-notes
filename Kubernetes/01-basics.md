Kubernetes is an open‑source container orchestration platform used to deploy, scale, manage, and heal containerized applications automatically.

> Why Kubernetes is Needed

Before K8s:
Applications ran on single servers, Manual deployment, No auto‑scaling, If a server died app went down

> With containers:

Easier packaging (Docker)
But managing hundreds or thousands of containers is hard

> Kubernetes solves this problem
It provides:

High availability, Auto‑scaling, Load balancing, Self‑healing, Rolling updates with zero downtime

> Kubernetes Architecture 
A Kubernetes cluster is a group of machines (nodes) that work together to run containerized applications.
It has two main components:

Control Plane (Master Node)
Worker Nodes

> **Control Plane (Master Node)**
The Control Plane is the brain of Kubernetes.
It is responsible for managing, scheduling, and maintaining the desired state of the cluster.
Key Responsibility:
Makes decisions about what should run, where it should run, and keeps the cluster healthy.

**API Server (kube-apiserver)**
Acts as the front door of the Kubernetes cluster
All commands and requests go through the API Server
Communicates using REST APIs

Key Points:

kubectl, UI, CI/CD tools → talk to API Server
Validates requests (authentication & authorization)
Stores cluster state in etcd
Central communication hub

📌 Without API Server, Kubernetes cannot function

**etcd**
A distributed key‑value database Stores entire cluster state
Stores:
Pod definitions, ConfigMaps, Secrets, Node information, Desired state vs current state

Key Points:
Source of truth for the cluster
Highly available & consistent
Losing etcd = losing cluster state

Backup of etcd is critical

**Scheduler (kube-scheduler)**
Decides which worker node a Pod should run on

How it works:

Finds nodes with enough CPU & memory
Applies rules like:

Node selectors
Affinity / Anti‑affinity
Taints & tolerations

Chooses the best node

Scheduler does not run pods — it only assigns them

**Controller Manager (kube-controller-manager)**

Runs multiple controllers
Constantly watches cluster state and fixes issues

Important Controllers:

Node Controller
ReplicaSet Controller
Deployment Controller
Job Controller

Example:

Desired Pods = 3
Running Pods = 2
Controller creates 1 new Pod

📌 This is how Kubernetes achieves self‑healing

> **Worker Nodes**
Worker nodes are where actual applications run.
Each worker node contains:

**kubelet**

Agent running on every worker node
Talks to the API Server

Responsibilities:

Receives Pod instructions
Pulls container images
Ensures containers are running
Reports node & pod status

kubelet manages Pods, not containers directly

**Container Runtime**

Software that actually runs containers

Examples:

Docker (older)
containerd (most common)
CRI‑O

Responsibilities:

Pull images
Start containers
Stop containers

📌 Kubernetes does not run containers itself

**kube-proxy**

Handles network communication
Enables Pod‑to‑Pod and Service‑to‑Pod traffic

Responsibilities:

IP tables rules
Load balancing
Service discovery

📌 Required for Services to work

> How Kubernetes Architecture Works (Flow)
Step‑by‑Step Example:
> 
User runs:
Shellkubectl apply -f deployment.yaml
Request goes to API Server--
API Server stores data in etcd--
Scheduler selects suitable worker node--
Controller Manager ensures desired replicas--

kubelet on worker node:
Pulls image--
Creates Pod--
Container runtime starts containers--
kube-proxy enables networking
