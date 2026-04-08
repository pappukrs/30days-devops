Perfect — Day 4 is where you start thinking like a real backend + DevOps engineer 🔥
Because now you separate:

❌ Code vs Config
✅ Runtime configuration (industry standard)

🧠 DAY 4 — CONFIGMAP vs SECRET
🔥 PROBLEM FIRST (WHY WE NEED THIS)

Right now your Node app might have:

const DB_URL = "mongodb://localhost:27017";

👉 This is BAD ❌

Because:

Hardcoded
Not flexible
Not secure
🎯 SOLUTION

👉 Move config OUT of code → into Kubernetes

🧱 1. CONFIGMAP (NON-SENSITIVE DATA)

👉 Used for:

API URLs
ports
feature flags
✅ Example
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  PORT: "3000"
  ENV: "production"
🔐 2. SECRET (SENSITIVE DATA)

👉 Used for:

passwords
tokens
DB credentials
✅ Example
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQ=

👉 cGFzc3dvcmQ= = base64("password")

⚠️ IMPORTANT

👉 Secret is:

base64 encoded (NOT encrypted ❌)
still slightly safer than plain text
🔥 DIFFERENCE (INTERVIEW GOLD)
Feature	ConfigMap	Secret
Data type	normal	sensitive
Encoding	plain	base64
Use case	config	credentials
🚀 NOW HANDS-ON (VERY IMPORTANT)
🧱 STEP 1 — CREATE CONFIGMAP
apiVersion: v1
kind: ConfigMap
metadata:
  name: video-config
data:
  PORT: "3000"
  APP_NAME: "video-app"
🧱 STEP 2 — USE IN DEPLOYMENT

Modify your deployment:

containers:
- name: backend
  image: pappukrs/devops-30-day2
  env:
    - name: PORT
      valueFrom:
        configMapKeyRef:
          name: video-config
          key: PORT
🧠 WHAT THIS DOES
ConfigMap → injects env → container
🚀 APPLY
kubectl apply -f configmap.yaml
kubectl apply -f deployment.yaml
🔍 VERIFY
kubectl exec -it <pod-name> -- env

👉 You should see:

PORT=3000
APP_NAME=video-app
🐧 LINUX COMMANDS (TODAY)
🔹 1. env
env

👉 Shows all environment variables

🔹 2. export
export NAME=pappu

👉 Sets env variable

Check:

echo $NAME
🔹 3. .bashrc

👉 File where env variables persist

nano ~/.bashrc

Add:

export MY_APP=video

Then:

source ~/.bashrc
🧠 REAL DEVOPS CONNECTION
Linux	Kubernetes
export VAR=value	ConfigMap
env	kubectl exec env
💣 DEBUG TASK (IMPORTANT)

Break it:

Change wrong key
See pod fail

Check:

kubectl describe pod <pod>
🎯 DAY 4 CHECKLIST

You are done if:

 ConfigMap created
 Deployment updated
 Env vars visible in pod
 Understand difference ConfigMap vs Secret
 Practiced env/export/.bashrc
🚀 INDUSTRY INSIGHT

👉 In real systems:

ConfigMap → app config
Secret → DB credentials
Both managed via GitOps
🔥 NEXT STEP

Tomorrow (Day 5):

👉 Ingress (REAL PRODUCTION ROUTING)

No more raw IPs
Domain-based routing
