
🧠 DAY 3 — SERVICES (CORE CONCEPT)

You already deployed pods (Day 2).
👉 Problem now:

❌ Pods are NOT accessible from outside
❌ Pod IP keeps changing

👉 Solution = Service

🧠 BIG IDEA
Pods (dynamic, changing)
   ↓
Service (stable)
   ↓
User / Internet
🔥 TYPES OF SERVICES
1️⃣ ClusterIP (DEFAULT)

👉 Internal only

Used for:
Frontend → Backend
Backend → DB
🧠 Example
frontend calls:
http://backend-service:3000

👉 Works only inside cluster

2️⃣ NodePort

👉 Opens port on node

http://<node-ip>:30000+
⚠️ Problems:
Hard to manage
Not used in production mostly
3️⃣ LoadBalancer (IMPORTANT 🚀)

👉 Creates cloud load balancer (GCP)

Internet → External IP → Service → Pod

👉 You already saw:

EXTERNAL-IP: 35.244.xx.xx
🎯 SUMMARY TABLE
Type	Access	Use
ClusterIP	internal	services communication
NodePort	semi-public	testing
LoadBalancer	public	real apps
🚀 NOW HANDS-ON (YOUR TASK)

We expose your Node.js backend

🧱 STEP 1 — SERVICE YAML
apiVersion: v1
kind: Service
metadata:
  name: video-backend-service
spec:
  selector:
    app: video-backend
  ports:
    - port: 80
      targetPort: 3000
  type: LoadBalancer
🧠 UNDERSTAND THIS (IMPORTANT)
Field	Meaning
selector	finds pods
port	external service port
targetPort	container port
🔥 CONNECTION LOGIC
Service selector → app=video-backend
Deployment label → app=video-backend

👉 MATCH = traffic flows
🚀 STEP 2 — APPLY
kubectl apply -f service.yaml
🚀 STEP 3 — CHECK
kubectl get svc

👉 Wait 1–2 min:

EXTERNAL-IP → assigned
🌍 STEP 4 — TEST
curl http://<external-ip>

OR open in browser

🧠 REAL FLOW (IMPORTANT)
User → External IP → LoadBalancer → NodePort → Pod
🐧 LINUX (TODAY COMMANDS)
🔹 1. ping
ping google.com

👉 Check:

network connectivity
DNS working
🔹 2. traceroute
traceroute google.com

👉 Shows:

path packets travel
network hops
Install if not present:
sudo apt install traceroute
🧠 REAL DEVOPS USE
Command	Use
ping	check connectivity
traceroute	debug network path
💣 DEBUG SCENARIOS (IMPORTANT)
❌ External IP not working

Check:

kubectl get pods
kubectl get svc
kubectl describe svc video-backend-service
❌ No response

Check:

port mismatch
container running?
logs
kubectl logs <pod>
🎯 DAY 3 CHECKLIST

You are done if:

 Service YAML created
 Service applied
 External IP assigned
 App accessible via browser
 Understood all 3 service types
 Used ping & traceroute
🚀 REAL INDUSTRY NOTE

👉 Companies rarely use LoadBalancer directly for everything
👉 They use:
