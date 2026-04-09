DAY 7 — MINI PROJECT (FULL DEVOPS PRACTICE)
🎯 Goal

Deploy a complete application on Kubernetes using:

Deployment ✅
Service ✅
ConfigMap / Secret ✅
Ingress ✅
Debugging ✅
💡 PROJECT IDEA (REALISTIC)
🎥 “Video Meeting App (Lite Version)”

Since your main project is video calling, we’ll simulate core structure:

🧱 ARCHITECTURE
User (Browser)
   ↓
Ingress (NGINX)
   ↓
Frontend (React / simple HTML)
   ↓
Backend (Node.js API)
   ↓
Database (MongoDB)
📦 COMPONENTS YOU WILL BUILD
1️⃣ Frontend
Simple UI
Calls backend /api

👉 Can be:

React OR simple HTML (start simple)
2️⃣ Backend (IMPORTANT)
Node.js (your existing)
API: /api/health
3️⃣ Database
MongoDB container
🔥 FULL IMPLEMENTATION PLAN
🧱 STEP 1 — DEPLOY DATABASE
MongoDB Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongo
        image: mongo
        ports:
        - containerPort: 27017
Mongo Service (ClusterIP)
apiVersion: v1
kind: Service
metadata:
  name: mongo-service
spec:
  selector:
    app: mongo
  ports:
    - port: 27017
🧠 Concept used:

✅ ClusterIP (internal communication)

🧱 STEP 2 — CONFIGMAP + SECRET
ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DB_HOST: mongo-service
  PORT: "3000"
Secret
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQ=
🧠 Concept used:

✅ ConfigMap
✅ Secret

🧱 STEP 3 — BACKEND DEPLOYMENT
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: pappukrs/devops-30-day2
        ports:
        - containerPort: 3000
        env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: DB_HOST
Backend Service
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
    - port: 80
      targetPort: 3000
🧠 Concepts used:

✅ Deployment
✅ ReplicaSet
✅ Service
✅ ConfigMap

🧱 STEP 4 — FRONTEND DEPLOYMENT

Use simple nginx frontend:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: nginx
        ports:
        - containerPort: 80
Frontend Service
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - port: 80
🧱 STEP 5 — INGRESS (MAIN MAGIC 🔥)
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 80

      - path: /api
        pathType: Prefix
        backend:
          service:
            name: backend-service
            port:
              number: 80
🧠 Concepts used:

✅ Ingress
✅ Routing
✅ Reverse proxy

💣 STEP 6 — DEBUG PRACTICE

Break things intentionally:

Wrong image ❌
Wrong env ❌
Wrong service name ❌

Then fix using:

kubectl logs
kubectl describe pod
kubectl get events
🐧 LINUX USAGE (ALL DAYS COMBINED)

Use:

ps aux
top
curl
grep
tail -f
env
🎯 FINAL RESULT

You will have:

Full app running on Kubernetes
Public access via Ingress
Backend + frontend + DB connected
Config managed properly
🚀 HOW TO STRUCTURE YOUR REPO
day7-project/
 ├── mongo/
 ├── backend/
 ├── frontend/
 ├── ingress.yaml
 ├── configmap.yaml
 ├── secret.yaml
 └── README.md
🧠 WHAT YOU CAN SAY IN INTERVIEW

“I deployed a full microservice-style application with frontend, backend, and database on Kubernetes, using ConfigMaps, Secrets, and Ingress for routing.”

🔥 This is very strong

🚀 NEXT STEP
