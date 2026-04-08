🧠 DAY 5 — INGRESS (CORE IDEA)
❌ Problem with LoadBalancer

Right now:

Each service = 1 external IP ❌
Expensive 💰
Not scalable
✅ Solution = INGRESS

👉 One entry point for all traffic

Internet
   ↓
Ingress (NGINX)
   ↓
/api → backend
/ → frontend
🔥 WHAT IS INGRESS?

👉 A reverse proxy + router inside Kubernetes

👉 It decides:

“Where should this request go?”

🧠 REVERSE PROXY (IMPORTANT)

Example:

User → /api → backend
User → / → frontend

👉 User doesn’t know internal services

🚀 STEP 1 — INSTALL NGINX INGRESS

Run:

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
⏳ Wait for it
kubectl get pods -n ingress-nginx

👉 Wait until:

Running ✅
🔍 Get external IP
kubectl get svc -n ingress-nginx

👉 You’ll see:

EXTERNAL-IP: 34.xx.xx.xx
🧱 STEP 2 — CREATE INGRESS YAML
🔥 Example (IMPORTANT)
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: video-ingress
spec:
  rules:
  - http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: video-backend-service
            port:
              number: 80
🧠 UNDERSTAND THIS
Field	Meaning
path: /api	route
service.name	where to send
port	service port
🔥 FLOW
User → /api → Ingress → backend service → pod
🚀 STEP 3 — APPLY
kubectl apply -f ingress.yaml
🔍 VERIFY
kubectl get ingress

👉 You’ll get:

ADDRESS: <external-ip>
🌍 TEST
curl http://<external-ip>/api
🐧 LINUX (NGINX BASICS)
🔹 What is NGINX?

👉 Web server + reverse proxy

Used in:

Kubernetes ingress
Production systems
🔹 Logs (VERY IMPORTANT)

Inside pod:

kubectl logs <nginx-ingress-pod> -n ingress-nginx
🔹 Common log use
kubectl logs <pod> | grep error

👉 Debug routing issues

💣 DEBUG SCENARIOS
❌ 404 Not Found

👉 Means:

path mismatch

Check:

kubectl describe ingress video-ingress
❌ No response

Check:

service name correct?
pods running?
kubectl get svc
kubectl get pods
❌ Ingress not getting IP

Wait or check:

kubectl get svc -n ingress-nginx
🎯 REAL INDUSTRY ARCHITECTURE
User
 ↓
Domain (example.com)
 ↓
Ingress (NGINX)
 ↓
Services
 ↓
Pods
🚀 WHY INGRESS IS IMPORTANT
Feature	Benefit
One IP	cost saving
Routing	clean architecture
SSL support	HTTPS
Domain support	production ready
🎯 DAY 5 CHECKLIST
 Ingress installed
 External IP available
 Routing working /api
 Logs checked
 Understood reverse proxy
🔥 BIG LEARNING

👉 LoadBalancer = simple
👉 Ingress = production

🚀 NEXT STEP

Day 6 = 🔥 Debugging master level

You’ll learn:

why pods fail
how to fix anything
