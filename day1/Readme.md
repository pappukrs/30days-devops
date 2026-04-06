DAY 1 — GKE + FIRST DEPLOY (DEEP + HANDS-ON)
🎯 Goal of Day 1

By end of today, you should:

Understand how Kubernetes actually works internally
Have a running cluster on Google Kubernetes Engine
Deploy your first app using YAML (not UI, not shortcuts)
Be comfortable with kubectl basics
🧠 PART 1 — CORE CONCEPT (30–40 min)

👉 Don’t skip this. This is what separates you from “tutorial DevOps”.

🔑 Kubernetes Mental Model

Think like this:

Cluster
 ├── Control Plane (brain)
 └── Nodes (machines)
        └── Pods (your apps)
🔥 Key Components (Must know today)
1. Cluster
Group of machines
Managed by GKE
2. Node
VM inside cluster
Runs your apps
3. Pod (MOST IMPORTANT)
Smallest deployable unit
Usually 1 container

👉 Real meaning:

“Pod = running instance of your app”

4. Deployment
Keeps pods alive
Handles scaling & updates
5. kubectl
CLI to talk to cluster
🛠️ PART 2 — HANDS-ON (STEP BY STEP)
✅ Step 1: Create GKE Cluster

Go to:
👉 Google Cloud Platform

Then:

Kubernetes Engine → Clusters → Create
Recommended config (save money 💰):
Mode: Autopilot (easy + cheaper for now)
Region: closest (asia-south1)
Nodes: auto-managed
✅ Step 2: Connect Cluster
gcloud container clusters get-credentials <cluster-name> --region <region>

Check:

kubectl get nodes

👉 If nodes visible = success ✅

✅ Step 3: Your FIRST Deployment (nginx)

Create file:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
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
        ports:
        - containerPort: 80
✅ Step 4: Apply it
kubectl apply -f deployment.yaml

Check:

kubectl get pods
✅ Step 5: Expose Service
kubectl expose deployment nginx-deployment \
  --type=LoadBalancer \
  --port=80
✅ Step 6: Get External IP
kubectl get svc

👉 Open IP in browser → nginx page

🐧 PART 3 — LINUX PRACTICE (MANDATORY)

Today commands:

ps aux
top
kill -9 <pid>
curl <your-service-ip>

👉 Understand:

Process vs container
Network call via curl
💥 PART 4 — BREAK & DEBUG (VERY IMPORTANT)

Now intentionally break:

👉 Edit YAML:

image: nginx:wrongtag

Apply again.

Now:

kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>

👉 You’ll see:

ImagePullError

🎯 This is REAL DevOps learning

📦 PART 5 — GIT (DON’T SKIP)

Push everything to:
👉 GitHub

Structure:

day-1/
 ├── deployment.yaml
 ├── service.yaml (optional)
 └── README.md
📝 README MUST INCLUDE:
What is Pod
What is Deployment
Steps you followed
Errors you faced
How you fixed
🧠 INDUSTRY INSIGHT (VERY IMPORTANT)

👉 Today you are doing manually.

In companies:

Nobody runs kubectl apply manually
Everything goes via:
Git → CI/CD → GitOps

You’ll learn that in Week 3.

🎯 END OF DAY CHECKLIST

You are done if:

 GKE cluster running
 nginx deployed
 Service exposed
 Can access via browser
 Broke something & fixed it
 Code pushed to GitHub
⚡ BONUS (IF EXTRA TIME)

👉 Deploy using command (no YAML)
kubectl create deployment nginx --image=nginx



______How to create deployments & service.yml from scratch------------------

Cluster = system (brain + workers)
Node = VM
Pod = where app actually runs ✅ (this is important)
Deployment = manages pods (not just “file”)
Service = exposes pods

👉 Think:

Deployment → creates Pods  
Service → exposes Pods  
🔥 NOW YOUR MAIN DOUBT

“How to create Deployment & Service YAML from scratch?”

This is VERY IMPORTANT SKILL (real DevOps)

🧱 1. DEPLOYMENT YAML (FROM SCRATCH)

Let’s build it step by step (not copy-paste blindly)

✅ MINIMUM REQUIRED STRUCTURE
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        ports:
        - containerPort: 80
🧠 HOW TO THINK (IMPORTANT)
🔹 apiVersion
apiVersion: apps/v1

👉 Which API version Kubernetes uses

🔹 kind
kind: Deployment

👉 What you are creating

🔹 metadata
metadata:
  name: my-app

👉 Name of deployment

🔹 replicas
replicas: 1

👉 How many pods you want

🔹 selector + labels (CRITICAL 🔥)
selector:
  matchLabels:
    app: my-app

👉 This tells:

“Which pods belong to this deployment”

🔹 template (actual pod definition)
template:
  metadata:
    labels:
      app: my-app

👉 Labels MUST match selector

🔹 container
containers:
- name: my-container
  image: nginx

👉 Your actual app

💡 GOLDEN RULE

👉 selector label == template label

If mismatch → deployment breaks ❌

🚀 2. SERVICE YAML (FROM SCRATCH)
✅ MINIMUM SERVICE
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 80
  type: LoadBalancer
🧠 BREAKDOWN
🔹 selector
selector:
  app: my-app

👉 This connects service → pods

🔹 ports
port: 80
targetPort: 80
Field	Meaning
port	service port
targetPort	pod port
🔹 type
type: LoadBalancer

👉 Makes it public

🔥 HOW THEY CONNECT (VERY IMPORTANT)
Deployment → Pods (label: app=my-app)

Service → selector (app=my-app)

👉 Service finds pods using labels
🎯 REAL FLOW
Service → matches label → routes traffic → Pod
🛠️ HOW TO CREATE FROM SCRATCH (REAL METHOD)

Instead of memorizing, use this trick:

⚡ Method 1: Generate YAML (BEST PRACTICE)
kubectl create deployment my-app --image=nginx --dry-run=client -o yaml

👉 This gives base YAML

⚡ For service:
kubectl expose deployment my-app --port=80 --type=LoadBalancer --dry-run=client -o yaml

👉 Then modify → commit → apply

🧠 INDUSTRY PRACTICE

Real engineers:

Don’t write YAML fully from memory
Generate → edit → reuse
🔥 YOUR TASK (IMPORTANT)

Create files:

deployment.yaml
name: video-backend
image: nginx (for now)
service.yaml
type: LoadBalancer
connect to deployment

Then run:

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
🎯 AFTER THIS

You will:

Understand YAML deeply
Not depend on tutorials
Think like DevOps engineer
🚀 NEXT

