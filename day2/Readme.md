DAY 2 — PODS vs DEPLOYMENT vs REPLICASET
🧠 CORE CONCEPT (VERY IMPORTANT)
🔹 1. Pod (actual running app)

👉 Smallest unit in Kubernetes

Pod = Container (your app)

👉 If pod dies → gone ❌

🔹 2. ReplicaSet (keeps pods alive)

👉 Ensures:

“I want 3 pods always running”

🔹 3. Deployment (what YOU use)

👉 Manages:

ReplicaSet
Updates
Scaling
🧠 RELATION (MUST REMEMBER)
Deployment → ReplicaSet → Pods
🎯 REAL MEANING
Thing	Role
Pod	runs app
ReplicaSet	keeps count
Deployment	controls everything
🚀 NOW HANDS-ON (YOUR REAL TASK)

We will deploy your Node.js backend

🛠️ STEP 1 — DO YOU HAVE DOCKER IMAGE?

👉 Tell me:

Do you already have Dockerfile?
Image built?
❗ If NOT → create Dockerfile

Example (Node.js):

FROM node:18

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["node", "index.js"]
✅ Build image
docker build -t your-dockerhub-username/video-backend .
✅ Push image
docker push your-dockerhub-username/video-backend
🧱 STEP 2 — CREATE DEPLOYMENT YAML (REAL)
🔥 YOUR REAL DEPLOYMENT
apiVersion: apps/v1
kind: Deployment
metadata:
  name: video-backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: video-backend
  template:
    metadata:
      labels:
        app: video-backend
    spec:
      containers:
      - name: backend
        image: your-dockerhub-username/video-backend
        ports:
        - containerPort: 3000
🚀 Apply
kubectl apply -f deployment.yaml
✅ Check
kubectl get pods
🧠 WHAT YOU SHOULD SEE
video-backend-xxxxx   Running
video-backend-yyyyy   Running

👉 2 pods (because replicas = 2)

🔥 STEP 3 — VERIFY DEPLOYMENT
kubectl get deployment
kubectl describe deployment video-backend
🧠 UNDERSTAND THIS OUTPUT

You’ll see:

ReplicaSet created
Pods managed

👉 This confirms:

Deployment → ReplicaSet → Pods
🐧 LINUX (IMPORTANT TODAY)
🔹 curl → test API
curl http://<pod-ip>:3000
🔹 netstat → check ports
netstat -tulnp
🔹 ss (modern version)
ss -tulnp
💣 BREAK & LEARN (MANDATORY)
❌ Break image
image: wrong-image-name
🔍 Debug
kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>

👉 Learn:

ImagePullBackOff
CrashLoopBackOff


🎯 TODAY CHECKLIST

You are done if:

 Docker image created
 Pushed to registry
 Deployment YAML created
 Pods running
 Replica concept clear
 Tested using curl